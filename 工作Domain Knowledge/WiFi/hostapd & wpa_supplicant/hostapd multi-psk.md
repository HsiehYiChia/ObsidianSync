---

---

### First patch

這是2004的patch, 已經跟現在的code差異很大了, 但裡面有講述了multi-psk的使用情境, 是要針對home-network而且不需要獨立radius server

[http://lists.infradead.org/pipermail/hostap/2004-September/008184.html](http://lists.infradead.org/pipermail/hostap/2004-September/008184.html)

### 2019 patch

這是Michal Kazior上的, 主要是新增

- hostapd_cli reload動態新增或刪除psk
- hostapd_cli all_sta時秀出keyid

[https://w1.fi/cgit/hostap/commit/?id=b08c9ad0c78d2c767d62e1d169560debe8db0ce6](https://w1.fi/cgit/hostap/commit/?id=b08c9ad0c78d2c767d62e1d169560debe8db0ce6)

[https://w1.fi/cgit/hostap/commit/?id=ec5c39a5574d4fbee983ae659ea0834bf48ea14d](https://w1.fi/cgit/hostap/commit/?id=ec5c39a5574d4fbee983ae659ea0834bf48ea14d)

[https://w1.fi/cgit/hostap/commit/?id=83c8608132f5e4b756aaac2df6ab06f6776f6642](https://w1.fi/cgit/hostap/commit/?id=83c8608132f5e4b756aaac2df6ab06f6776f6642)


### hostapd key handshake



### hostapd multi psk 實作

從hostapd.wpa_psk_file讀出multi psk

```c
hostapd_setup_wpa_psk() -> hostapd_config_read_wpa_psk()
```


當AP收到從STA送來的EAPOL KEY msg 2/4, 進到這個function, 他會

- 去一一使用能用的PSK, 並算出PTK與MIC
- 比對自己算出的MIC與STA傳來的MIC, 若吻合則成功, 不吻合則用下一個PSK算

```c
SM_STATE(WPA_PTK, PTKCALCNEGOTIATING)
{
...
        /* WPA with IEEE 802.1X: use the derived PMK from EAP
         * WPA-PSK: iterate through possible PSKs and select the one matching
         * the packet */
        for (;;) {
...
                if (wpa_derive_ptk(sm, sm->SNonce, pmk, pmk_len, &PTK,
                                   owe_ptk_workaround == 2) < 0)
                        break;

                if (mic_len &&
                    wpa_verify_key_mic(sm->wpa_key_mgmt, pmk_len, &PTK,
                                       sm->last_rx_eapol_key,
                                       sm->last_rx_eapol_key_len) == 0) {
                        if (sm->PMK != pmk) {
                                os_memcpy(sm->PMK, pmk, pmk_len);
                                sm->pmk_len = pmk_len;
                        }
                        ok = 1;
                        break;
                }
...
}
```

