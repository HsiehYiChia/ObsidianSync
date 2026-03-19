---

---
```c
mt76_rx_complete() -> ieee80211_rx_list() -> __ieee80211_rx_handle_8023()   # H/W path
mt76_rx_complete() -> ieee80211_rx_list() -> __ieee80211_rx_handle_packet() # S/W path
```

HDR translate by H/W

```c
__ieee80211_rx_handle_8023()->ieee80211_rx_8023()->netif_receive_skb()
```

HDR translate by S/W

```c
__ieee80211_rx_handle_packet()->ieee80211_prepare_and_rx_handle()->ieee80211_invoke_rx_handlers() -> ieee80211_rx_handlers() -> 
```
