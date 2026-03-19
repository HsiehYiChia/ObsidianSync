---

---
### What is the DHCP DORA process in detail?

The best way to remember how does DHCP process works is by remembering the Acronym **DORA**, which translates to,

D – Discover     O – Offer     R – Request    A – Acknowledge

It looks like below.

![](https://getlabsdone.com/wp-content/uploads/2019/07/ezgif.com-optimize.gif)



![[Untitled 32.png]]

###  References

[https://getlabsdone.com/the-dhcp-process-explained/](https://getlabsdone.com/the-dhcp-process-explained/)

[https://documentation.meraki.com/General_Administration/Tools_and_Troubleshooting/Using_Packet_Capture_to_Troubleshoot_Client-side_DHCP_Issues](https://documentation.meraki.com/General_Administration/Tools_and_Troubleshooting/Using_Packet_Capture_to_Troubleshoot_Client-side_DHCP_Issues)

[https://prod-files-secure.s3.us-west-2.amazonaws.com/7d7e5b12-f37c-4d87-a647-306c5d6ad4c0/11d0f5bc-c197-49d3-a395-0a107b91b334/DHCP.cap?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667T6XZOEV%2F20251207%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251207T190749Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD5HzKO%2FLKRNV%2F96OQ1BUsGuBrTBw81%2B54zyokSuCTU%2BwIgdslTDe8EcB7RxuYE2%2BuqqqR59WOsUqJGkGhh9ZJ%2FKakqiAQIlP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCOZ2Jmph3uUNEweFSrcA93XFo9tMEXRKhow8F18q%2B8ZxkzTk9kBImdpS7jFI5Jwizuk08XufWILIE%2FdmHxf02RRIaayF%2FvgEmW3a0SRMWY2GwGYuGiZ17g0U7QPldKiKr%2FNBNhUXmpUHxrkL90rwcHXMHMOupLRBKP2BhxywUums3QBf3NrusaGOOpGl565I%2BJ1y%2B4Rl4%2BIDzHZe7WzIDayXG6%2F7HeazIkTb7J3uYhcegQlrrqnFHU0Xhj7A3sO%2BlH5d%2BnMNGwgEjF%2BzlGRMNLItcGgB8VZovWwd2ZUwJl29%2B7CNvBYScIF5GIHw%2FXN5MXS2XHnJXXWG4pyBi2zym6kfnMt8sjTcUbgYVOC%2BxmcP1BjBRnPfxTPCltIP5Of2%2Bw%2BnAm1273zfm04wsGv6wcZjNdPDjGi%2BPsEOluHcIjYCoHJsFbmSVDVCwOxd1JDwgcrXTjsy9wypkM2%2BF4KKBIR9ytHA8YNp1K9Tz8UF1mvrLHlJ4WgZ%2BkAgOd%2FGjlgGqc80ygtQ0K%2BVA2VJAEBoWi5ULcM8mrl%2FwJRNRrCeXEie8eSk0T%2BxN2mBHwTSxbwCuv8c7qqfS3WrVPQdxBqvuej0HFRPhgEz8dmMNc6cWS%2BMR2dMWb1dwbVd%2B%2BKxv2q0UwvHvE6Hglc%2FC6%2BMOCV18kGOqUBqTMfiFGUSR77H14q%2F5ukIzBXo1zbZJI9LhoLFsXUQLCEdQCJBTPR5Ff%2Bsf9aVP074pGWGz8UQJEQb0HBs97ZknZrESDl%2BV0wOvgpAKobZwaX0ds3GO5qBLVYTMHWscCaUOc%2F6O2zW%2BZKoKrbhqh9494s613ZAzYynfJ3sjx4SCfe79w2Vvtuy%2F1Vgye1CUAoF8ILA6c30pyFOmVwzokNFL%2F8kJZT&X-Amz-Signature=26e9132cfc89a543ad679a479b39ac9d7ef846712b3b182764c6b9e41283944e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject](https://prod-files-secure.s3.us-west-2.amazonaws.com/7d7e5b12-f37c-4d87-a647-306c5d6ad4c0/11d0f5bc-c197-49d3-a395-0a107b91b334/DHCP.cap?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667T6XZOEV%2F20251207%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251207T190749Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD5HzKO%2FLKRNV%2F96OQ1BUsGuBrTBw81%2B54zyokSuCTU%2BwIgdslTDe8EcB7RxuYE2%2BuqqqR59WOsUqJGkGhh9ZJ%2FKakqiAQIlP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCOZ2Jmph3uUNEweFSrcA93XFo9tMEXRKhow8F18q%2B8ZxkzTk9kBImdpS7jFI5Jwizuk08XufWILIE%2FdmHxf02RRIaayF%2FvgEmW3a0SRMWY2GwGYuGiZ17g0U7QPldKiKr%2FNBNhUXmpUHxrkL90rwcHXMHMOupLRBKP2BhxywUums3QBf3NrusaGOOpGl565I%2BJ1y%2B4Rl4%2BIDzHZe7WzIDayXG6%2F7HeazIkTb7J3uYhcegQlrrqnFHU0Xhj7A3sO%2BlH5d%2BnMNGwgEjF%2BzlGRMNLItcGgB8VZovWwd2ZUwJl29%2B7CNvBYScIF5GIHw%2FXN5MXS2XHnJXXWG4pyBi2zym6kfnMt8sjTcUbgYVOC%2BxmcP1BjBRnPfxTPCltIP5Of2%2Bw%2BnAm1273zfm04wsGv6wcZjNdPDjGi%2BPsEOluHcIjYCoHJsFbmSVDVCwOxd1JDwgcrXTjsy9wypkM2%2BF4KKBIR9ytHA8YNp1K9Tz8UF1mvrLHlJ4WgZ%2BkAgOd%2FGjlgGqc80ygtQ0K%2BVA2VJAEBoWi5ULcM8mrl%2FwJRNRrCeXEie8eSk0T%2BxN2mBHwTSxbwCuv8c7qqfS3WrVPQdxBqvuej0HFRPhgEz8dmMNc6cWS%2BMR2dMWb1dwbVd%2B%2BKxv2q0UwvHvE6Hglc%2FC6%2BMOCV18kGOqUBqTMfiFGUSR77H14q%2F5ukIzBXo1zbZJI9LhoLFsXUQLCEdQCJBTPR5Ff%2Bsf9aVP074pGWGz8UQJEQb0HBs97ZknZrESDl%2BV0wOvgpAKobZwaX0ds3GO5qBLVYTMHWscCaUOc%2F6O2zW%2BZKoKrbhqh9494s613ZAzYynfJ3sjx4SCfe79w2Vvtuy%2F1Vgye1CUAoF8ILA6c30pyFOmVwzokNFL%2F8kJZT&X-Amz-Signature=26e9132cfc89a543ad679a479b39ac9d7ef846712b3b182764c6b9e41283944e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)
