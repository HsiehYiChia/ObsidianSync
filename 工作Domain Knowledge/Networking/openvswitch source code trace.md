---

---
RX

netdev_port_receive()->ovs_vport_receive()处理接收报文，ovs_flow_key_extract()

函数生成 flow 的 key 内容用以接下来进行流表匹配，最后调用 ovs_dp_process_packet()

[https://blog.csdn.net/nb_zsy/article/details/107893255](https://blog.csdn.net/nb_zsy/article/details/107893255)
