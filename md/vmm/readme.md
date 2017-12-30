[返回目录](/README.md)

虚拟内存管理
===========================

虚拟内存管理起到的作用有：

  虚拟内存管理，可以`控制`物理内存的`访问权限`；

  ```
  利用 CPU 模式和 MMU 的内存保护机制实现，内核地址空间也被保护起来，防止在用户模式下执行错误的指令意外改写内核数据。这样，`恶意代码`的破坏能力受到了限制。
  ```

  虚拟内存管理，可以让每个进程有`独立的地址空间`；

  ```
  不同进程中的同一个VA被MMU映射到不同的PA，并且在某一个进程中访问任何地址都不可能访问到另外一个进程的数据。这样，`恶意代码`无法访问另一个进程任何数据。

  每个进程，都认为自己独占`整个虚拟地址空间`，这样链接器和加载器的实现会比较容易，不必考虑各进程的地址范围是否冲突。
  ```


- 用 ps 命令查看当前终端下的`进程`：

```bash
ps
```

```vim
  PID TTY          TIME CMD
 8543 pts/4    00:00:00 bash        # bash 进程的 id
31520 pts/4    00:00:00 ps
```

- 查看此 **bash 进程**的`虚拟地址空间`：

  `/proc 目录`中的文件，是由内核虚拟出来的文件系统。当前系统中运行的每个`进程`在 /proc 下都有一个`子目录`，目录名就是进程的id。查看目录下的文件可以得到该进程的相关信息。

  例如，查看 maps：

```bash
cat /proc/8543/maps
```

```vim
00400000-004f4000 r-xp 00000000 fd:00 4849671                            /bin/bash
006f3000-006f4000 r--p 000f3000 fd:00 4849671                            /bin/bash
006f4000-006fd000 rw-p 000f4000 fd:00 4849671                            /bin/bash
006fd000-00703000 rw-p 00000000 00:00 0
016d3000-018b7000 rw-p 00000000 00:00 0                                  [heap]
...
7f1ecc02a000-7f1ecc02b000 rw-p 0000b000 fd:00 11407931                   /lib/x86_64-linux-gnu/libnss_files-2.23.so
7f1ecc02b000-7f1ecc031000 rw-p 00000000 00:00 0
7f1ecc031000-7f1ecc03c000 r-xp 00000000 fd:00 11407941                   /lib/x86_64-linux-gnu/libnss_nis-2.23.so
...
7f1ecda3b000-7f1ecda3c000 rw-p 00026000 fd:00 11407804                   /lib/x86_64-linux-gnu/ld-2.23.so
7f1ecda3c000-7f1ecda3d000 rw-p 00000000 00:00 0
7ffe0ded1000-7ffe0def2000 rw-p 00000000 00:00 0                          [stack]
7ffe0dff7000-7ffe0dff9000 r--p 00000000 00:00 0                          [vvar]
7ffe0dff9000-7ffe0dffb000 r-xp 00000000 00:00 0                          [vdso]
ffffffffff600000-ffffffffff601000 r-xp 00000000 00:00 0                  [vsyscall]
```

  或者：

```bash
pmap 8543
```

```vim
8543:   bash
0000000000400000    976K r-x-- bash
00000000006f3000      4K r---- bash
00000000006f4000     36K rw--- bash
00000000006fd000     24K rw---   [ anon ]
00000000016d3000   1952K rw---   [ anon ]
00007f1ecbe1f000     44K r-x-- libnss_files-2.23.so
...
00007f1ecd814000      4K rw--- libtinfo.so.5.9
00007f1ecd815000    152K r-x-- ld-2.23.so
00007f1ecd9ff000    100K r---- bash.mo
00007f1ecda18000     16K rw---   [ anon ]
00007f1ecda31000     28K r--s- gconv-modules.cache
00007f1ecda38000      8K rw---   [ anon ]
00007f1ecda3a000      4K r---- ld-2.23.so
00007f1ecda3b000      4K rw--- ld-2.23.so
00007f1ecda3c000      4K rw---   [ anon ]
00007ffe0ded1000    132K rw---   [ stack ]
00007ffe0dff7000      8K r----   [ anon ]
00007ffe0dff9000      8K r-x--   [ anon ]
ffffffffff600000      4K r-x--   [ anon ]
 total            30044K
```
