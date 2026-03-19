---

---
#plume #gre

Normal Wi-Fi frames that are transmitted over the air, have room for only (3) MAC addresses:1) RA: MAC address of the wireless receiver (next hop over Wi-Fi)2) TA: MAC address of the wireless transmitter (source over Wi-Fi)3) SA or DA: Depends on direction: Client => AP this is Packet original source MAC address, and in the AP => Client direction this is the Packet final destination address

When bridging packets across multiple nodes (think of multiple Ethernet switches which are daisy chained), the original source and destination MAC addresses must be kept intact. Due to the above three address frame operation of Wi-Fi, this isn't normally possible.

Example:

**[server]** –- LANeth --- **[XBGW]** –- 5GHz --- **[pod] **--- 5GHz --- **[client]**

A packet sent from ***client*** to ***server***, would look like this:

- Original _ethernet_ packet created by app on [client]: **SA** = [client], **DA** = [server]
- [client] => [pod]: **RA** = [pod], **TA** = [client], **DA** = [server]
- [pod] => [XBGW]: **RA** = [XBGW], **TA** = [pod], **DA** = [server] ***note: we lost the original client MAC here***
- [XBGW] => [server]: **SA** = [pod], **DA** = [server]
- Final *ethernet* packet received by the server: **SA** = [pod], **DA** = [server]

A packet sent from ***server*** to ***client***, would look like this:

- Original *ethernet* packet created by app on [server]: **SA** = [server], **DA** = [client]
- [server] => [XBGW]: **SA** = [server], **DA** = [client]
- [XBGW] => [pod]: **RA** = [pod], **TA** = [XBGW], **DA** = [client] ***note: we lost the original ***<u>***server***</u>*** MAC here***
- [pod] => [client]: **RA** = [client], **TA** = [pod], **DA** = [client]
- Final *ethernet* packet received by the app on the client: **SA** = [pod], **DA** = [client]

From both examples, you can see that the original vs. final L2 Ethernet packets do not match, and one of the addresses (client or server) are lost. This breaks the fundamental basics of L2 bridging. Consider Forwarding/MAC learning tables within switches, and you can understand the issues this would cause, not to mention firewalls, ARP tables, etc: It just doesn't work. Also note that it gets much more complex if there are additional hops.---------

By using GRE to "tunnel" the packets over Wi-Fi, you are adding a static header to the original Ethernet frame, sending it over Wifi, then removing that static header. This means the original Ethernet SA and DA addresses are saved. A GRE encapsulated packet looks like this over Wi-Fi:

1. RA: MAC address of the wireless receiver (next hop over Wi-Fi)
2. TA: MAC address of the wireless transmitter (source of the Wi-Fi packet)
3. SA or DA: Depends on direction, can be client or server
4. ETH Protocol: IP
5. Source IP: The IP address of the Wi-Fi device transmitting the packet (from 2 above)
6. Dest IP: The IP address of the Wi-Fi device receiving the packet (from 1 above)
7. IP Proto: GRE
8. [GRE encapsulated] DA: Final Destination MAC address of the original Ethernet packet
9. [GRE encapsulated] SA: Original Source MAC address of the original Ethernet packet
10. [GRE encapsulated] ...rest of the original Ethernet packet...
11. using GRE encapsulation, the following happens:
- The transmitter of the packet (XB to pod, or pod to XB) will add 1-7 from above (38 bytes), allowing the original Ethernet frame to remain unmodifiedThe receiver of the packet (XB to pod, or pod to XB) will remove 1-7 from above (38 bytes), resulting in the original Ethernet frame unmodifiedNote: This is why larger MTU over Wi-Fi is needed, because we're adding 38 bytes to all frames sent -- and also why hardware acceleration plays a role in terms of adding/removing the GRE headers when accelerating packets.*We do use GRE today in our retail and other customer deployments*

It's worth noting, that full implementation of GRE can result in additional processing, because there is the possibility of using GRE sequence numbers to deal with lost/dropped frames, as well as CRC checks to handle corrupted data. This extra processing can result in impact to performance. At Plume, we have implemented a small light weight kernel driver which implements what we call, is `Static GRE`: This is where we fore-go the sequence number and CRC checking, due to Wi-Fi's reliability, and removing all requirements of GRE processing, except inserting the "static" GRE header on transmitted Wi-Fi packets, and stripping off the static GRE header on received Wi-Fi packets going between XB/pods. This is what's in use today on both XE1 and XE2 pods.
