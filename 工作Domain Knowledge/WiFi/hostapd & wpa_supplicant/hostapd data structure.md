---

---


### relation

- a `hostapd_iface` may have multiple `hostapd_bss_config`, `hostapd_iface` 感覺有點像是一個main interface, 如果要有mbss, 那就會在main interface下開bss存在hostapd_bss_config
- 從下方的code可以清楚看出reload_iface會reload所有bss

![[Untitled 22.png]]

- 
- `hostapd_interface` 感覺像是最上層的structure, 整個hostapd只有一個, 可以容納多個`hostapd_iface (radio)`, 且會依照driver種類不同存很多callback

![[Untitled 23.png]]


```c
/**
 * struct hostapd_data - hostapd per-BSS data structure
 */
struct hostapd_data {
        struct hostapd_iface *iface;
        struct hostapd_config *iconf;
        struct hostapd_bss_config *conf;
```

```c
/**
 * struct hostapd_iface - hostapd per-interface data structure
 */
struct hostapd_iface {
        struct hapd_interfaces *interfaces;
        void *owner;
        char *config_fname;
        struct hostapd_config *conf;
        char phy[16]; /* Name of the PHY (radio) */
...
        size_t num_bss;
        struct hostapd_data **bss;
```

```c
/**
 * struct hostapd_config - Per-radio interface configuration
 */
struct hostapd_config {
        struct hostapd_bss_config **bss, *last_bss;
        size_t num_bss;
```

```c
/**
 * struct hostapd_bss_config - Per-BSS configuration
 */
struct hostapd_bss_config {
        char iface[IFNAMSIZ + 1];
        char bridge[IFNAMSIZ + 1];
        char vlan_bridge[IFNAMSIZ + 1];
        char wds_bridge[IFNAMSIZ + 1];
```

![[Untitled 24.png]]