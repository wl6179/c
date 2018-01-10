[返回目录](/README.md)

解锁技能 —— 查阅 虚拟内存、物理内存
============================

查看物理内存
----------

随时可以通过`操作系统`（OS级别）的 shell 命令查看得到。

```bash
cat /proc/meminfo

MemTotal:        8076000 kB
MemFree:         1125068 kB
MemAvailable:    4096180 kB
Buffers:          212512 kB
Cached:          3399536 kB
SwapCached:            0 kB
Active:          5094596 kB
Inactive:        1494568 kB
Active(anon):    2791388 kB
......
```

查看虚拟内存
----------

可以肯定的说，`C语言程序`中看到的所有内存地址，都是 VA 虚拟内存的地址！

c程序，也是属于计算机的`应用层`，所以程序里输出、得出的任何内存地址，其实都是虚拟内存地址 VA。而想要看到物理内存地址 PA 则需要深入到`内核`才能看到。
