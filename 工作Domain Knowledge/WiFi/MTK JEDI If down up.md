---

---


```javascript
static inline INT VIRTUAL_IF_INIT(VOID *pAd, VOID *pDev)
 {
         RT_CMD_INF_UP_DOWN InfConf = {
                 virtual_if_init_handler,
                 virtual_if_deinit_handler,
                 virtual_if_up_handler,
                 virtual_if_down_handler,
                 pDev
         };

         if (RTMP_COM_IoctlHandle(pAd, NULL, CMD_RTPRIV_IOCTL_VIRTUAL_INF_INIT,
                                                          0, &InfConf, 0) != NDIS_STATUS_SUCCESS)
                 return -1;

         return 0;
 }

INT virtual_if_init_handler(VOID *dev)
 {
...
                 /* must use main net_dev */
                 if (mt_wifi_open(pAd->net_dev) != 0) {
                         VIRTUAL_IF_DEC(pAd);
                         MTWF_LOG(DBG_CAT_CFG, DBG_SUBCAT_ALL, DBG_LVL_TRACE,
                                          ("mt_wifi_open return fail!\n"));
                         return NDIS_STATUS_FAILURE;
                 }
...
 }


int mt_wifi_open(VOID *dev)
{
...
/* Chip & other init */
         if (mt_wifi_init(pAd, mac, hostname) == FALSE)
                 goto err;
...
}

int mt_wifi_init(VOID *pAdSrc, RTMP_STRING *pDefaultMac, RTMP_STRING *pHostName)
{
/*for software system initialize*/
         if (rtmp_sys_init(pAd, pHostName) != TRUE)
                 goto err2;

         Status = WfInit(pAd);
         /* initialize MLME*/
         Status = MlmeInit(pAd);

				 tr_ctl_init(pAd);

```

AP wdev hook

```javascript
struct wifi_dev_ops ap_wdev_ops = {
         .tx_pkt_allowed = ap_tx_pkt_allowed,
         .fp_tx_pkt_allowed = ap_fp_tx_pkt_allowed,
         .send_data_pkt = ap_send_data_pkt,
         .fp_send_data_pkt = fp_send_data_pkt,
         .send_mlme_pkt = ap_send_mlme_pkt,
         .tx_pkt_handle = ap_tx_pkt_handle,
         .legacy_tx = ap_legacy_tx,
         .ampdu_tx = ap_ampdu_tx,
         .frag_tx = ap_frag_tx,
         .amsdu_tx = ap_amsdu_tx,
         .fill_non_offload_tx_blk = ap_fill_non_offload_tx_blk,
         .fill_offload_tx_blk = ap_fill_offload_tx_blk,
         .mlme_mgmtq_tx = ap_mlme_mgmtq_tx,
         .mlme_dataq_tx = ap_mlme_dataq_tx,
 #ifdef CONFIG_ATE
         .ate_tx = mt_ate_tx,
 #endif
 #ifdef VERIFICATION_MODE
         .verify_tx = verify_pkt_tx,
 #endif
         .ieee_802_11_data_tx = ap_ieee_802_11_data_tx,
         .ieee_802_3_data_tx = ap_ieee_802_3_data_tx,
         .rx_pkt_allowed = ap_rx_pkt_allowed,
         .rx_ps_handle = ap_rx_ps_handle,
         .rx_pkt_foward = ap_rx_pkt_foward,
         .ieee_802_3_data_rx = ap_ieee_802_3_data_rx,
         .ieee_802_11_data_rx = ap_ieee_802_11_data_rx,
         .find_cipher_algorithm = ap_find_cipher_algorithm,
         .mac_entry_lookup = mac_entry_lookup,
         .media_state_connected = media_state_connected,
         .ioctl = rt28xx_ap_ioctl,
         .open = ap_inf_open,
         .close = ap_inf_close,
         .linkup = ap_link_up,
         .linkdown = ap_link_down,
         .conn_act = ap_conn_act,
         .disconn_act = wifi_sys_disconn_act,
 };
```