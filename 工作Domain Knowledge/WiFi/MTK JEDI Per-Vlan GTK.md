---

---
# Task Status

- [x] 設定 hostapd EAP, 並架設RADIUS server且開啟VLAN設定
- [x] Driver可以新增VLAN interface
- [x] Driver可以取得每個VLAN interface的GTK
- [x] Driver取得WTBL, 且將GTK設定到WTBL裡
- [x] Driver在TX時依照VLAN用不同的GTK加密 (確認封包是否有VLAN? 若沒則需用netdev來選GTK)
- [x] Driver在RX時依照VLAN用不同的GTK解密 (與DE確認RX decryption的行為)

# Todo

- [x] 確認DYNAMIC_VLAN打開後為什麼ping不通?
- [x] 將vlan_gtk相關struct整成一個, 而非零散的資料
- [x] 確認STA是與rai0還是rai0.1做 4 way? cfg80211_new_sta 硬塞rai0.1上去, 無效

---

# 筆記


### 20201023

**Tx**: Driver將封包打上tag vlan_id=1送到 `rai0`, kernel會摘掉tag並送到 `rai0.1`

**Rx**: Kernel會透過`rai0.1` 將封包送出, vlan的hard xmit會打上tag並送往 `rai0`

### 20200930

- pkt tagged> vlan_tag進到kernel後被摘掉 > 封包進到rai0.1 > Driver不知道tag >透過netdev判斷
- pkt tagged > vlan_tag進到kernel後沒被摘 > 封包進到rai0 > Driver知道tag > 透過vlan_tag判斷
- pkt untagged >進kernel不做事 > 

![[Untitled 25.png]]


---

# 測試情境

- AP開rai0與rai0.1
- STA連線上rai0.1
- AP發出Broadcast `untagged` ICMP request → Driver用rai0 bmc →  預期STA收不到  →  ping不通
- AP發出Broadcast `tagged` ICMP request → Driver用rai0.1 bmc送 → STA能解的開 → ping通

### 相關修改

[http://gerrit.mediatek.inc:8080/c/neptune/wlan_driver/jedi/+/2850280](http://gerrit.mediatek.inc:8080/c/neptune/wlan_driver/jedi/+/2850280) 

[http://gerrit.mediatek.inc:8080/c/neptune/wlan_daemon/hostapd/+/2975674](http://gerrit.mediatek.inc:8080/c/neptune/wlan_daemon/hostapd/+/2975674)

# 要開的CONFIG

VLAN_SUPPORT

DYNAMIC_VLAN

IWCOMMAND_CFG80211_SUPPORT

# 要修改的地方

- Enable IWCOMMAND_CFG80211_SUPPORT, package/mtk/drivers/mt_wifi/Makefile,config.in

```c
static struct wireless_dev *CFG80211_WdevAlloc(
		pWdev->wiphy->interface_modes |= BIT(NL80211_IFTYPE_AP_VLAN);

```

# VLAN_SUPPORT與DYNAMIC_VLAN歷史

kunal

```c
[2020/9/25 上午 11:01] Kunal Kumar: 
for upper layer the data packet is tagged 
[2020/9/25 上午 11:04] Yichia Hsieh (謝易家): 
Only unicast data is tagged? Is BMC data tagged?
[2020/9/25 上午 11:04] Yichia Hsieh (謝易家): 
Does hostapd create virtual interface for each vlan?
[2020/9/25 上午 11:06] Yichia Hsieh (謝易家): 
Can DYNAMIC_VLAN be enabled along with VLAN_SUPPORT?
[2020/9/25 下午 01:18] Kunal Kumar: 
Only unicast data is tagged? Is BMC data tagged?
Yes, dynamic vlan is only applicable to unicast packets

Does hostapd create virtual interface for each vlan?
any vlan id can be assigned to a virtual interface & for multiple vlans multiple virtual interfaces need to be created

Can DYNAMIC_VLAN be enabled along with VLAN_SUPPORT? 
No, Dynamic vlan is applicable in case of (AP <--> STA connection) but static vlan is applicable in WDS connection
[2020/9/25 下午 01:38] Yichia Hsieh (謝易家): 
ok,  thanks for your info
[2020/9/25 下午 01:38] Yichia Hsieh (謝易家): 
it can let me know the history and context of vlan feature in driver


[2020/10/15 下午 02:14] Kunal Kumar: 
Connect AP to STA, after connection, follow below steps: 
1. Set VLAN ID:				iwpriv rai0 set dvlan=2
2. Create VLAN interface:	vconfig add rai0 2
               Above command would create rai0.2 as virtual interface
               Don't add rai0.2 to the bridge
3. Assign an IP address to the virtually created interface(rai0.2), which should be different from the bridge subnet:
				ifconfig rai0.2  192.168.2.1
              Assuming bridge has the ip address 10.10.10.254
4. Make the newly created virtual interface up :
				ifconfig  rai0.2 up
5. Configure STA with same subnet IP Address
6. Ping should work between STA & virtual interface(rai0.2)
```

shailesh

```c
[2020/10/16 下午 02:13] Shailesh Dixit: 
for earlier implementation of dynamic vlan we didn't use hostapd to create virtual interface..
[2020/10/16 下午 02:13] Yichia Hsieh (謝易家): 
but when pkt with tag vid=1 dev=rai0 is send to kernel, it is drop and not sending to rai0.1 as expected
[2020/10/16 下午 02:15] Shailesh Dixit: 
Is rai0.1 connected to bridge..
[2020/10/16 下午 02:15] Yichia Hsieh (謝易家): 
hostapd will create bridge for each vlan intf
[2020/10/16 下午 02:16] Yichia Hsieh (謝易家): 
rai0.1 to brrai0.1
rai0.2 to brrai0.2
etc
[2020/10/16 下午 02:17] Yichia Hsieh (謝易家): 
if we create interface by iwpriv, hostapd will not send gtk to driver
[2020/10/16 下午 02:18] Shailesh Dixit: 
creating virtual interface through vlanconfig type of app..
[2020/10/16 下午 02:18] Shailesh Dixit: 
for sending gtk.. we need to patch hostapd to pass vlan id along with gtk..
[2020/10/16 下午 02:19] Shailesh Dixit: 
and in driver based on vlan id different gtk are installed for different interfaces..
[2020/10/16 下午 02:19] Shailesh Dixit: 
in your case since all vlan interfaces are connected with different bridge..
[2020/10/16 下午 02:19] Shailesh Dixit: 
either packet should be indicated with correct net device..
[2020/10/16 下午 02:20] Shailesh Dixit: 
net device of rai0.1 .
[2020/10/16 下午 02:20] Shailesh Dixit: 
For our Dynamic vlan test.. there was net device of rai0 only..
[2020/10/16 下午 02:21] Shailesh Dixit: 
although we create virtual interfaces through vlan config..
[2020/10/16 下午 02:21] Shailesh Dixit: 
and all interfaces are connected through br-lan..
[2020/10/16 下午 02:21] Shailesh Dixit: 
Although I am not sure how UI uses Dynamic vlan..
[2020/10/16 下午 02:21] Shailesh Dixit: 
you can check with UI how they create dynamic vlan configuration..
[2020/10/16 下午 02:35] Yichia Hsieh (謝易家): 
For our Dynamic vlan test.. there was net device of rai0 only..
    > if we tagged a packet with vid 1 and send to rai0, how will that pkt be handle by kernel? Will it be send to rai0.1?
    > Can you give me some hint?
[2020/10/16 下午 02:36] Shailesh Dixit: 
bridge on receiving packet check for vlan tagged packet and sent to appropriate vlan interface..
[2020/10/16 下午 02:36] Shailesh Dixit: 
packet you indicate to kernel will contain vlan tag..
```

