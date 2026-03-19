---

---
Any problems for use?  Actually hostapd will remove this interface and create interface again when Wi-Fi up. Or you can remove/unregister it through NL80211_CMD_DEL_INTERFACE: iw dev wlan0" del

This is a common behavior controlled by native Kernel/mac80211 to probe device. When mac80211 drivers (ath10k or mt76) register device they will call ieee80211_register_hw  mt76_register_device -> ieee80211_register_hw  -> register wlan0

```c
if (local->hw.wiphy->interface_modes & BIT(NL80211_IFTYPE_STATION) &&
                !ieee80211_hw_check(hw, NO_AUTO_VIF)) {
                        struct vif_params params = {0};
                        result = ieee80211_if_add(local, "wlan%d", NET_NAME_ENUM, NULL,
                                                              NL80211_IFTYPE_STATION, &params);
                        if (result)
                                    wiphy_warn(local->hw.wiphy,
                                                   "Failed to add default virtual iface\n");
            }
```
