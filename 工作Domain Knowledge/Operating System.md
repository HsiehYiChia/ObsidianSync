---

---
# Operating system

- Segment
- Memory map IO vs Port map IO
- Thread vs Process
- race condition
- mutex, semaphore
- Interrupt
暫停並保存當前程式的資訊 > context switch > 根據interrupt id查interrupt vector table取得ISR > 執行ISR > 回到原本程式的地方繼續執行

[https://mropengate.blogspot.com/2015/01/operating-system-ch2-os-structures.html](https://mropengate.blogspot.com/2015/01/operating-system-ch2-os-structures.html)

- Buttom half and top half

[https://www3.cs.stonybrook.edu/~dongyoon/cse506-f19/lecture/lec13-int-bottom-half.pdf](https://www3.cs.stonybrook.edu/~dongyoon/cse506-f19/lecture/lec13-int-bottom-half.pdf)

[https://www.oreilly.com/library/view/linux-device-drivers/0596000081/ch09s05.html](https://www.oreilly.com/library/view/linux-device-drivers/0596000081/ch09s05.html)

[https://dwei1224.wordpress.com/2018/01/26/linux-kernel-about-bottom-half-softirq-tasklet-work-queue/](https://dwei1224.wordpress.com/2018/01/26/linux-kernel-about-bottom-half-softirq-tasklet-work-queue/)

[http://clhjoe.blogspot.com/2012/07/linux-interrupt.html](http://clhjoe.blogspot.com/2012/07/linux-interrupt.html)

- tasklet

[http://unicornx.github.io/2016/02/12/20160212-lk-drv-tasklet/](http://unicornx.github.io/2016/02/12/20160212-lk-drv-tasklet/)
