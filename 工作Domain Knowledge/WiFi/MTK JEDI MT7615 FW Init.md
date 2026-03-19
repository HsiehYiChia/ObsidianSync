---

---
## Host driver

in `hw_ctrl/hw_init.c`

```c
WfMcuInit -> WfMcuSysInit (設定register)
          -> WfMcuHwInit (FW download)


WfMcuHwInit()
{
...
         if (ops->prepare_fwdl_img)
                 ops->prepare_fwdl_img(pAd);

         ret = NICLoadRomPatch(pAd);
...
         ret = NICLoadFirmware(pAd);
}

mt7615_fw_prepare()
{

}
```

## FW Entry Point

in `wifi_uni_mac_7615/Service/wifi_init.c`

```c
wifi_normal_operation_entry()
{
...
while(1)
     {
         prOsMsg = (P_KAL_MSG_T)kalDequeMsg(INDX_WIFI);

         if (WIFI_Event_Dispatcher(KAL_GET_MSG_ID(prOsMsg), KAL_GET_MSG_DATA(prOsMsg)) < 0)
         {
             kalFreeMsg(prOsMsg);

             /* ensure no pending messages remained */
             while (kalGetMsgQueSize(INDX_WIFI) > 0)
             {
                 prOsMsg = kalDequeMsg(INDX_WIFI);
                 kalFreeMsg(prOsMsg);
             }
             break;
         }

         kalFreeMsg(prOsMsg);
     }
...
}
```

---

