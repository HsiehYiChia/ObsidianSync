---

---
handle_assoc_cb→hostapd_set_wds_sta→i802_set_wds_sta


```c
static void handle_assoc_cb
{
...
  hostapd_set_sta_flags(hapd, sta);

	if (!(sta->flags & WLAN_STA_WDS) && sta->pending_wds_enable) {
		wpa_printf(MSG_DEBUG, "Enable 4-address WDS mode for STA "
			   MACSTR " based on pending request",
			   MAC2STR(sta->addr));
		sta->pending_wds_enable = 0;
		sta->flags |= WLAN_STA_WDS;
	}

	if (sta->flags & (WLAN_STA_WDS | WLAN_STA_MULTI_AP)) {
		int ret;
		char ifname_wds[IFNAMSIZ + 1];

		wpa_printf(MSG_DEBUG, "Reenable 4-address WDS mode for STA "
			   MACSTR " (aid %u)",
			   MAC2STR(sta->addr), sta->aid);
		ret = hostapd_set_wds_sta(hapd, ifname_wds, sta->addr,
					  sta->aid, 1);
		if (!ret)
			hostapd_set_wds_encryption(hapd, sta, ifname_wds);
	}
```

```c
int hostapd_set_wds_sta(struct hostapd_data *hapd, char *ifname_wds,
			const u8 *addr, int aid, int val)
{
	const char *bridge = NULL;

	if (hapd->driver == NULL || hapd->driver->set_wds_sta == NULL)
		return -1;
	if (hapd->conf->wds_bridge[0])
		bridge = hapd->conf->wds_bridge;
	else if (hapd->conf->bridge[0])
		bridge = hapd->conf->bridge;
	return hapd->driver->set_wds_sta(hapd->drv_priv, addr, aid, val,
					 bridge, ifname_wds);
}
```

```c
static int i802_set_wds_sta(void *priv, const u8 *addr, int aid, int val,
			    const char *bridge_ifname, char *ifname_wds)
{
...
  if (val) {
		if (!if_nametoindex(name)) {
			if (nl80211_create_iface(drv, name,
						 NL80211_IFTYPE_AP_VLAN,
						 bss->addr, 1, NULL, NULL, 0) <
			    0)
				return -1;
			if (bridge_ifname &&
			    linux_br_add_if(drv->global->ioctl_sock,
					    bridge_ifname, name) < 0)
				return -1;

			os_memset(&event, 0, sizeof(event));
			event.wds_sta_interface.sta_addr = addr;
			event.wds_sta_interface.ifname = name;
			event.wds_sta_interface.istatus = INTERFACE_ADDED;
			wpa_supplicant_event(bss->ctx,
					     EVENT_WDS_STA_INTERFACE_STATUS,
					     &event);
		}
		if (linux_set_iface_flags(drv->global->ioctl_sock, name, 1)) {
			wpa_printf(MSG_ERROR, "nl80211: Failed to set WDS STA "
				   "interface %s up", name);
		}
		return i802_set_sta_vlan(priv, addr, name, 0);
	}
```