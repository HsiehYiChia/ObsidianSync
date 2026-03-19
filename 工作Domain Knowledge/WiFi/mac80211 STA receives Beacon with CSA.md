---

---
```c
ieee80211_rx_mgmt_beacon -> ieee80211_sta_process_chanswitch -> ieee80211_queue_work(&local->hw, &ifmgd->chswitch_work) -> ieee80211_chswitch_work -> ieee80211_vif_use_reserved_context -> ieee80211_vif_use_reserved_switch

```



```c
[   81.503805] ieee80211_sta_process_chanswitch:
[   81.508202] ieee80211_sta_process_chanswitch: reserve_chanctx iftype=2
[   81.514752] ieee80211_sta_process_chanswitch: reserve_chanctx iftype=3
[   81.521294] ieee80211_sta_process_chanswitch: reserve_chanctx iftype=3
[   81.527827] ieee80211_sta_process_chanswitch: set csa params iftype=2
[   81.534279] ieee80211_sta_process_chanswitch: set csa params iftype=3
[   81.540729] ieee80211_sta_process_chanswitch: set csa params iftype=3
[   81.547174] ieee80211_sta_process_chanswitch: chan switch notify for iftype=2
[   81.554343] ieee80211_sta_process_chanswitch: chan switch notify for iftype=3
[   81.561513] ieee80211_sta_process_chanswitch: chan switch notify for iftype=3
[   81.568688] ieee80211_sta_process_chanswitch:
[   81.573134] ieee80211_sta_process_chanswitch:
[   81.651547] ieee80211_sta_process_chanswitch:
[   81.753855] ieee80211_sta_process_chanswitch:
[   81.856255] ieee80211_sta_process_chanswitch:
[   81.958674] ieee80211_sta_process_chanswitch:
[   82.062484] ieee80211_sta_process_chanswitch:
[   82.163501] ieee80211_sta_process_chanswitch:
[   82.265835] ieee80211_sta_process_chanswitch:
[   82.368222] ieee80211_sta_process_chanswitch:
[   82.470627] ieee80211_sta_process_chanswitch:
[   82.573076] ieee80211_sta_process_chanswitch:
[   82.675433] ieee80211_sta_process_chanswitch:
[   82.777837] ieee80211_sta_process_chanswitch:
[   82.880233] ieee80211_sta_process_chanswitch:
[   82.983387] ieee80211_sta_process_chanswitch:
[   83.006996] ieee80211_chswitch_work: 1--->
[   83.011103] ieee80211_chswitch_work: 2--->
[   83.015212] ieee80211_vif_use_reserved_context: reserved_ready=true iftype=2
[   83.022272] ieee80211_vif_use_reserved_context: reserved_ready=true iftype=3
[   83.029330] ieee80211_vif_use_reserved_context: reserved_ready=true iftype=3
[   83.036386] ieee80211_vif_use_reserved_switch: n_assigned=3 n_reserved=3 n_ready=3
[   83.059702] ieee80211_vif_chanctx_reservation_complete: NL80211_IFTYPE_AP csa_finalize_work
[   83.068082] ieee80211_vif_use_reserved_switch: reservation_complete iftype=3
[   83.075185] ieee80211_vif_chanctx_reservation_complete: NL80211_IFTYPE_AP csa_finalize_work
[   83.083559] ieee80211_vif_use_reserved_switch: reservation_complete iftype=3
[   83.090616] ieee80211_vif_chanctx_reservation_complete: NL80211_IFTYPE_STATION chswitch_work
[   83.099070] ieee80211_vif_use_reserved_switch: reservation_complete iftype=2
[   83.106129] ieee80211_vif_use_reserved_switch: assign
[   83.111196] ieee80211_vif_use_reserved_switch: free
[   83.116081] ieee80211_vif_use_reserved_switch: free finish
[   83.121573] ieee80211_chswitch_work: 2<---
[   83.125678] ieee80211_chswitch_work: 1<---
[   83.129844] ieee80211_sta_process_chanswitch:
[   83.134526] ieee80211_chswitch_work: 1--->
[   83.138627] ieee80211_chswitch_work: 1<---
[   83.187444] ieee80211_sta_process_chanswitch:
```
