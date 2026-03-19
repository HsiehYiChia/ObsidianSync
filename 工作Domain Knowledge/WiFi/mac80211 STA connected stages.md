---

---

If I’m a AP interface

```c
hapd
hostapd_mgmt_rx() -> ieee802_11_mgmt() -> handle_assoc() -> add_associated_sta() -> hostapd_sta_add() -> wpa_driver_nl80211_sta_add() -> NL80211_CMD_NEW_STATION ->

mac80211
nl80211_new_station() -> ieee80211_add_station() -> sta_apply_parameters() -> sta_info_move_state(sta, IEEE80211_STA_AUTHORIZED)
```

If I’m a STA interface

```c
ieee80211_rx_mgmt_assoc_resp() -> ieee80211_assoc_success() -> sta_info_move_state(sta, IEEE80211_STA_AUTHORIZED) -> cfg80211_send_layer2_update()
```


when TX

```c
static struct sk_buff *ieee80211_build_hdr
{
        /*
         * Drop unicast frames to unauthorised stations unless they are
         * EAPOL frames from the local station.
         */
        if (unlikely(!ieee80211_vif_is_mesh(&sdata->vif) &&
                     (sdata->vif.type != NL80211_IFTYPE_OCB) &&
                     !multicast && !authorized &&
                     (cpu_to_be16(ethertype) != sdata->control_port_protocol ||
                      !ether_addr_equal(sdata->vif.addr, skb->data + ETH_ALEN)))) {
#ifdef CONFIG_MAC80211_VERBOSE_DEBUG
                net_info_ratelimited("%s: dropped frame to %pM (unauthorized port)\n",
                                    sdata->name, hdr.addr1);
#endif

                I802_DEBUG_INC(local->tx_handlers_drop_unauth_port);

                ret = -EPERM;
                goto free;
        }
```


when RX

```c
static int ieee80211_802_1x_port_control(struct ieee80211_rx_data *rx)
{
        if (unlikely(!rx->sta || !test_sta_flag(rx->sta, WLAN_STA_AUTHORIZED)))
                return -EACCES;

        return 0;
}
```