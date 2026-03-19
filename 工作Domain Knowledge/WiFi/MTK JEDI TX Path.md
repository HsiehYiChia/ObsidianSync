---

---

## TX Flow (High level)

1. network stack to wireless driver (rt28xx_packet_xmit)

2. Kick-out the data into Host HW Ring (ge_tx_pkt_deq_func, queuing method by chip cap.)

3. HW release the packet (for cut-through mechanism only, in tx_free_event_handler)


## Key Functions

從kernel stack接收到data封包, 送進data queue(SW)的過程

```c
rt28xx_packet_xmit() -> RTMPSendPackets() -> wdev_tx_pkts() -> send_data_pkt() 
-> ap_tx_pkt_allowed() -> ap_send_data_pkt() -> enq_dataq_pkt() -> ge_enq_dataq_pkt()
```

dequeue出封包, 會在tx_pkt_handle判斷封包類型(data, mgmt, cntl), 然後再送進Host HW ring

```c
ge_tx_pkt_deq_func() -> tx_pkt_handle() -> ap_tx_pkt_handle() -> kickout_data_tx()

ap_tx_pkt_handle()->ap_amsdu_tx()->ap_ieee_802_11_data_tx()
                                 ->ap_build_802_11_header()
		                             ->ap_fill_non_offload_tx_blk()
	                               ->asic_hw_tx()
		
		
			arch_ops->hw_tx()
				
				mtd_write_tmac_info_by_host()
```

## Tx block diagram


1. 7615有4個queue: CMD, FW, TxRing1, TxRing2. 每個queue都是一個Ring. TxRing2是DBDC的時後才會用
2. 每個封包都會有token_id,
3. 只傳TXD的好處是, 因為PSE的容量有限, 如果只存放TXD的話, 可以節省PSE的空間 (在Aggregate或是封包很多的時後有好處)
4. Ring size決定DMA能queue的大小, token_id pool大小決定driver能queue多少packet

**[Tx Flow]**

5. Driver將資料的TXD傳送到SW queue裡
6. 將TXD送到 CR4 Queue->PDMA0->PSE->CR4
7. CR4回復TXD done, PDMA0發出done interrupt, TxRing的 cell能夠被free掉(data沒有被free掉)
8. CR4決定要發, 將TXD送到PLE
9. PLE決定要發, 透過PDMA0將剩下的data送過來
10. 送出去的時候會透過RX path送回TXFREE event(帶token_id) (藉由RX path來送event是因為前面ring裡面已經釋放掉cell了, driver無法得知何時真正被送出)
11. Driver藉由token_id將data釋放



### Decide A4 PKT

```c
#define IS_ENTRY_A4(_x)                                 ((_x)->a4_entry != 0)
#define GET_ENTRY_A4(_x)                                ((_x)->a4_entry)
#define SET_ENTRY_A4(_x, _type)                 ((_x)->a4_entry = _type)
```

### TX_BLK_FLAGS

```c
TX_BLK_TEST_FLAG
TX_BLK_SET_FLAG

typedef enum TX_BLK_FLAGS {
         fTX_bRtsRequired = (1 << 0),
         fTX_bAckRequired = (1 << 1),
         fTX_bPiggyBack = (1 << 2),
         fTX_bHTRate = (1 << 3),
         fTX_bWMM = (1 << 4),
         fTX_bAllowFrag = (1 << 5),
         fTX_bMoreData = (1 << 6),
         fTX_bRetryUnlimit = (1 << 7),
         fTX_bClearEAPFrame = (1 << 8),
         fTX_bApCliPacket = (1 << 9),
         fTX_bSwEncrypt = (1 << 10),
         fTX_bWMM_UAPSD_EOSP = (1 << 11),
         fTX_bWDSEntry = (1 << 12),
         fTX_bDonglePkt = (1 << 13),
         fTX_bMeshEntry = (1 << 14),
         fTX_bWPIDataFrame = (1 << 15),
         fTX_bClientWDSFrame = (1 << 16),
         fTX_bTdlsEntry = (1 << 17),
         fTX_AmsduInAmpdu = (1 << 18),
         fTX_ForceRate = (1 << 19),
         fTX_CT_WithTxD = (1 << 20),
         fTX_CT_WithoutTxD = (1 << 21),
         fTX_DumpPkt = (1 << 22),
         fTX_HDR_TRANS = (1 << 23),
         fTX_MCU_OFFLOAD = (1 << 24),
         fTX_MCAST_CLONE = (1 << 25),
         fTX_HIGH_PRIO = (1 << 26),
 #ifdef A4_CONN
         fTX_bA4Frame = (1 << 27),
 #endif
         fTX_HW_AMSDU = (1 << 28),
         fTX_bNoRetry = (1 << 29),
         fTX_bAteTxsRequired = (1 << 30),
         fTX_bAteAgg = (1 << 31)
 } TX_BLK_FLAGS;
```