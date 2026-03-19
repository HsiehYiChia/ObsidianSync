---

---
- SIGTERM會留時間讓process做deinit動作, 並且通知child process
- SIGKILL會把process馬上殺掉, 如果該process有child process則會留下zombie process

這篇文章裡有個漫畫很生動~

[https://linuxhandbook.com/sigterm-vs-sigkill/](https://linuxhandbook.com/sigterm-vs-sigkill/)