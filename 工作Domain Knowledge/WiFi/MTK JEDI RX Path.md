---

---

## RX Flow (High level)

1. HW receive the packet from air

## Key Functions

```c
asic_rx_pkt_process() -> mt_rx_pkt_process() ->rx_data_handler() -> asic_trans_rxd_into_rxblk() 
-> rx_packet_process() -> dev_rx_802_11_data_frm()/dev_rx_802_3_data_frm() 
-> check_rx_pkt_allowed()-> ap_rx_pkt_allowed()

```

### HW HDR_TRANS RX Flow

```c
rx_packet_process() -> dev_rx_802_3_data_frm() -> ap_ieee_802_3_data_rx() -> rx_802_3_data_frm_announce() 
-> indicate_802_3_pkt() -> announce_or_forward_802_3_pkt() -> ops->rx_pkt_foward() -> ap_rx_pkt_foward()
```

### SW HDR_TRANS + HW deAMSDU RX Flow

```c
rx_packet_process() -> dev_rx_802_11_data_frm() -> ap_ieee_802_11_data_rx() -> rx_data_frm_announce()

ap_ieee_802_11_data_rx()
{
         if (pRxBlk->AmsduState) {
                 struct _RXD_BASE_STRUCT *rx_base = NULL;

                 rx_base = (struct _RXD_BASE_STRUCT *)pRxBlk->rmac_info;

                 if (rx_base->RxD1.HdrOffset == 1) {
                         pData += 2;
                         pRxBlk->DataSize -= 2;
                 }
                 pData += LENGTH_802_3;
                 pRxBlk->DataSize -= LENGTH_802_3;
         }
...
                 if (RX_BLK_TEST_FLAG(pRxBlk, fRX_HDR_TRANS))
                         rx_802_3_data_frm_announce(pAd, pEntry, pRxBlk, wdev);
                 else
                         rx_data_frm_announce(pAd, pEntry, pRxBlk, wdev);
}

rx_data_frm_announce()
{
                 if (RX_BLK_TEST_FLAG(pRxBlk, fRX_AMPDU) /*&& (pAd->CommonCfg.bDisableReordering == 0)*/)
                         indicate_ampdu_pkt(pAd, pRxBlk, wdev_idx);
                 else if (RX_BLK_TEST_FLAG(pRxBlk, fRX_AMSDU))
                         indicate_amsdu_pkt(pAd, pRxBlk, wdev_idx);
                 else
 #endif /* DOT11_N_SUPPORT */
                         if (RX_BLK_TEST_FLAG(pRxBlk, fRX_ARALINK))
                                 indicate_agg_ralink_pkt(pAd, pEntry, pRxBlk, wdev->wdev_idx);
                         else
                                 indicate_802_11_pkt(pAd, pRxBlk, wdev_idx);
}
```


**RX packet path**

0. PSE triggers Packet Processor to receive RX packets.

1. Packet Processor sends RX packets into PDMA 2 RX port.

2. Packet Processor sends RX packets* into PDMA 0 RX port.

3. PDMA 0 sends skb_buf_id to RX Token Write Back unit.

3.1. PDMA 0 RX DMA would pre-fetch DMA RXD, and get
skb_buf_id.

**Re-order buffer management**

4. RX Token Write Back unit informs skb_buf_id data to CR4.

4.1. RX Token Write Back unit interrupts CR4 after
defined conditions satisfied.

4.2. CR4 would fetch skb_buf_id information via
specified APB register window.

5. CR4 sends re-order buffer management packet to host
driver.

5.1 Host driver would release host memory allocations
due to this message.


### RX_BLK

> [!tip] 💡
> /*
All Rx routines use RX_BLK structure to hande rx events
It is very important to build pRxBlk attributes
1. pHeader pointer to 802.11 Header
2. pData pointer to payload including LLC (just skip Header)
3. set payload size including LLC to DataSize
4. set some flags with RX_BLK_SET_FLAG()
*/


### dump RX packet

```c
hex_dump("rx_data", rx_blk->pData, rx_blk->MPDUtotalByteCnt);



void hex_dump(char *str, UCHAR *pSrcBufVA, UINT SrcBufLen)
{
#ifdef DBG
    hex_dump_with_lvl(str, pSrcBufVA, SrcBufLen, DBG_LVL_TRACE);
#endif /* DBG */
}
```

### Test Flag

```c
if (RX_BLK_TEST_FLAG(pRxBlk, fRX_AMPDU))
if (RX_BLK_TEST_FLAG(pRxBlk, fRX_AMSDU))
if (RX_BLK_TEST_FLAG(pRxBlk, fRX_HDR_TRANS))
```


### RXD

```c
typedef struct GNU_PACKED _RXD_BASE_STRUCT {
         /* DWORD 0 */
         struct _RMAC_RXD_0 RxD0;
         /* DWORD 1 */
         struct _RMAC_RXD_1 RxD1;
         /* DWORD 2 */
         struct _RMAC_RXD_2 RxD2;
         /* DWORD 3 */
         /* struct rmac_rxd_3_normal RxD3;//do unify */
         struct _RMAC_RXD_3 RxD3;
 } RXD_BASE_STRUCT, *PRXD_BASE_STRUCT;
```