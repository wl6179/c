[返回目录](/README.md)

第四部 乾达婆，百变

![第四部 乾达婆，百变](/ig/4.jpg)


链接
===========================

一步编译多个文件

```bash
gcc main.c stack.c queue.c hello.c -o a.out
```

注意，文件顺序的调整，是会影响到代码的编排顺序的，如：

```bash
gcc hello.c stack.c queue.c main.c -o a.out
```

也分可以多步编译：

```bash
gcc -c hello.c
gcc -c stack.c
gcc -c queue.c
gcc -c main.c
gcc hello.o stack.o queue.o main.o -o main
```

用nm命令查看目标文件的符号表:

```bash
gcc -c hello.c
nm hello.o
```

通过 readelf 命令可以查看二进制文件信息：

```bash
readelf -a addmuniu.o       // 或者 a.out
```

```bash
ELF 头：
  Magic：   7f 45 4c 46 02 01 01 00 00 00 00 00 00 00 00 00
  类别:                              ELF64
  数据:                              2 补码，小端序 (little endian)
  版本:                              1 (current)
  OS/ABI:                            UNIX - System V
  ABI 版本:                          0
  类型:                              REL (可重定位文件)
  系统架构:                          Advanced Micro Devices X86-64
  版本:                              0x1
  入口点地址：               0x0
  程序头起点：          0 (bytes into file)
  Start of section headers:          2480 (bytes into file)
  标志：             0x0
  本头的大小：       64 (字节)
  程序头大小：       0 (字节)
  Number of program headers:         0
  节头大小：         64 (字节)
  节头数量：         13
  字符串表索引节头： 10

节头：
  [号] 名称              类型             地址              偏移量
       大小              全体大小          旗标   链接   信息   对齐
  [ 0]                   NULL             0000000000000000  00000000
       0000000000000000  0000000000000000           0     0     0
  [ 1] .text             PROGBITS         0000000000000000  00000040
       00000000000002ef  0000000000000000  AX       0     0     1
  [ 2] .rela.text        RELA             0000000000000000  00000588
       0000000000000390  0000000000000018   I      11     1     8
  [ 3] .data             PROGBITS         0000000000000000  00000330
       0000000000000004  0000000000000000  WA       0     0     4
  [ 4] .bss              NOBITS           0000000000000000  00000334
       0000000000000000  0000000000000000  WA       0     0     1
  [ 5] .rodata           PROGBITS         0000000000000000  00000338
       000000000000002f  0000000000000000   A       0     0     8
  [ 6] .comment          PROGBITS         0000000000000000  00000367
       0000000000000035  0000000000000001  MS       0     0     1
  [ 7] .note.GNU-stack   PROGBITS         0000000000000000  0000039c
       0000000000000000  0000000000000000           0     0     1
  [ 8] .eh_frame         PROGBITS         0000000000000000  000003a0
       0000000000000058  0000000000000000   A       0     0     8
  [ 9] .rela.eh_frame    RELA             0000000000000000  00000918
       0000000000000030  0000000000000018   I      11     8     8
  [10] .shstrtab         STRTAB           0000000000000000  00000948
       0000000000000061  0000000000000000           0     0     1
  [11] .symtab           SYMTAB           0000000000000000  000003f8
       0000000000000150  0000000000000018          12     9     8
  [12] .strtab           STRTAB           0000000000000000  00000548
       000000000000003c  0000000000000000           0     0     1
Key to Flags:
  W (write), A (alloc), X (execute), M (merge), S (strings), l (large)
  I (info), L (link order), G (group), T (TLS), E (exclude), x (unknown)
  O (extra OS processing required) o (OS specific), p (processor specific)

There are no section groups in this file.

本文件中没有程序头。

重定位节 '.rela.text' 位于偏移量 0x588 含有 38 个条目：
  偏移量          信息           类型           符号值        符号名称 + 加数
00000000004f  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
00000000005f  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
00000000006f  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
00000000007f  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
00000000008f  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
00000000009f  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
0000000000af  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
0000000000bf  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
0000000000cf  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
0000000000df  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
0000000000ef  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
00000000011c  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
00000000012c  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
00000000013c  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
00000000014c  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
00000000015c  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
00000000016c  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
00000000017c  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
00000000018c  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
00000000019c  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
0000000001ac  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
0000000001bc  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
0000000001db  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
0000000001eb  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
0000000001fb  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
00000000020b  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
00000000021b  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
00000000022b  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
00000000023b  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
00000000024b  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
00000000025b  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
00000000026b  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
00000000027b  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
0000000002bb  00050000000a R_X86_64_32       0000000000000000 .rodata + 0
0000000002c5  000c00000002 R_X86_64_PC32     0000000000000000 __isoc99_scanf - 4
0000000002d0  000a00000002 R_X86_64_PC32     0000000000000000 amuniu - 4
0000000002df  00050000000a R_X86_64_32       0000000000000000 .rodata + 8
0000000002e9  000d00000002 R_X86_64_PC32     0000000000000000 printf - 4

重定位节 '.rela.eh_frame' 位于偏移量 0x918 含有 2 个条目：
  偏移量          信息           类型           符号值        符号名称 + 加数
000000000020  000200000002 R_X86_64_PC32     0000000000000000 .text + 0
000000000044  000200000002 R_X86_64_PC32     0000000000000000 .text + 294

The decoding of unwind sections for machine type Advanced Micro Devices X86-64 is not currently supported.

Symbol table '.symtab' contains 14 entries:
   Num:    Value          Size Type    Bind   Vis      Ndx Name
     0: 0000000000000000     0 NOTYPE  LOCAL  DEFAULT  UND
     1: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS addmuniu.c
     2: 0000000000000000     0 SECTION LOCAL  DEFAULT    1
     3: 0000000000000000     0 SECTION LOCAL  DEFAULT    3
     4: 0000000000000000     0 SECTION LOCAL  DEFAULT    4
     5: 0000000000000000     0 SECTION LOCAL  DEFAULT    5
     6: 0000000000000000     0 SECTION LOCAL  DEFAULT    7
     7: 0000000000000000     0 SECTION LOCAL  DEFAULT    8
     8: 0000000000000000     0 SECTION LOCAL  DEFAULT    6
     9: 0000000000000000     4 OBJECT  GLOBAL DEFAULT    3 global_cattle
    10: 0000000000000000   660 FUNC    GLOBAL DEFAULT    1 amuniu
    11: 0000000000000294    91 FUNC    GLOBAL DEFAULT    1 main
    12: 0000000000000000     0 NOTYPE  GLOBAL DEFAULT  UND __isoc99_scanf
    13: 0000000000000000     0 NOTYPE  GLOBAL DEFAULT  UND printf

No version information found in this file.
```
