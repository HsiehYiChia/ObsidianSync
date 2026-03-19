---

---
這篇很詳細

[https://www.itread01.com/content/1546916583.html](https://www.itread01.com/content/1546916583.html)

```c
ioctl(skfd, SIOCGIFFLAGS, &ifr) -> dev_ioctl() -> dev_change_flags() -> ... ->
 __dev_open() -> ndo_open()
```