---

---
###  介紹

通用路由封装协议GRE（Generic Routing Encapsulation）可以对某些网络层协议（如IPX、ATM、IPv6、AppleTalk等）的数据报文进行封装，使这些被封装的数据报文能够在另一个网络层协议（如IPv4）中传输。GRE提供了将一种协议的报文封装在另一种协议报文中的机制，是一种三层隧道封装技术，使报文可以通过GRE隧道透明的传输，解决异种网络的传输问题。

GRE（Generic Routing Encapsulation，通用路由封裝）協議是對某些網絡層協議（IPX, AppleTalk, IP,etc.）的數據報文進行封裝，使這些被封裝的數據報文能夠在另一個網絡層協議（如IP）中傳輸。這是GRE最初的定義，最新的GRE封裝規範，已經**可以封裝二層數據幀**了，如PPP幀、MPLS等。在RFC2784中，**GRE的定義是“X over Y”，X和Y可以是任意的協議**。 GRE真的變成了“通用路由封裝”了。

### 封裝/解封裝過程

![[Untitled 30.png]]

- 傳輸協議（Transport Protocol / Delivery Protocol）：負責對封裝後的報文進行轉發的協議稱為傳輸協議。
- 封裝協議（Encapsulation Protocol）：上述的 GRE 協議稱為封裝協議，也稱為運載協議（Carrier Protocol）。
- 乘客協議（Passenger Protocol）：封裝前的報文協議稱為乘客協議。
- 淨荷（Payload Packet）：系統收到的需要封裝和路由的數據報稱為淨荷。

**報文封裝和解封裝過程：**
GRE封裝報文時，封裝前的報文稱為淨荷，封裝前的報文協議稱為乘客協議，然後先封裝GRE頭部，GRE成為封裝協議，也叫運載協議，最後負責對封裝後的報文進行轉發的協議稱為傳輸協議。
1）路由器從連接私網的接口接收到報文後，檢查報文頭中的目的IP地址字段，在路由表查找出接口，發現出接口是隧道接口，將報文發送給隧道模塊進行處理。
2）隧道模塊接收到報文後首先根據乘客協議的類型和當前GRE隧道配置的校驗和參數，對報文進行GRE封裝，添加GRE報頭。

3）路由器给报文添加传输协议报头即IP报头（公网地址）；该IP报头的源地址就是隧道源地址，目的地址就是隧道目的地址。

4）路由器根据新添加的IP报头目的地址，在路由表中查找相应的出接口并发送报文，封装后的报文将在公网中传输。

5、接收端路由器从连接公网的接口收到报文后，首先分析IP报文头，如果发现协议类型字段的值为47，表示协议为GRE，于是出接口将报文交给GRE模块处理。GRE模块去掉IP报文头和GRE报文头，并根据GRE报文头的协议类型字段，发现此报文的乘客协议为私网中运行的协议，于是将报文交给该协议处理。



# **GRE 的实现原理**

![](https://img-blog.csdnimg.cn/2019021210300295.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0ptaWxr,size_16,color_FFFFFF,t_70)

### gre tunnel vs gretap

![[Untitled 31.png]]

[https://david-waiting.medium.com/a-beginners-guide-to-generic-routing-encapsulation-fb2b4fb63abb](https://david-waiting.medium.com/a-beginners-guide-to-generic-routing-encapsulation-fb2b4fb63abb)


### references

[https://zhuanlan.zhihu.com/p/103214510](https://zhuanlan.zhihu.com/p/103214510)

[https://blog.csdn.net/Jmilk/article/details/114242161](https://blog.csdn.net/Jmilk/article/details/114242161)

[https://zhuanlan.zhihu.com/p/225724747](https://zhuanlan.zhihu.com/p/225724747)

[https://blog.csdn.net/yezing/article/details/47152377](https://blog.csdn.net/yezing/article/details/47152377)

[https://blog.csdn.net/sinat_20184565/article/details/83280247](https://blog.csdn.net/sinat_20184565/article/details/83280247)

[https://david-waiting.medium.com/a-beginners-guide-to-generic-routing-encapsulation-fb2b4fb63abb](https://david-waiting.medium.com/a-beginners-guide-to-generic-routing-encapsulation-fb2b4fb63abb)