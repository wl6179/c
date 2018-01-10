[返回目录](/README.md)

解锁技能 —— 查看一切源文件特征
============================

统计所有文件源代码行数
----------

先查看所有 .c 后缀的文件的行（`文件个数`）的统计。

find . -name "*.c" | wc -l

```bash
18
```

再查看各个 .c 后缀文件的代码行数。

find . -name "*.c" -exec wc {} -l \;

```bash
10 ./maths.c
8 ./return.c
8 ./helloworld.c
17 ./pointstruct.c
21 ./factorial.c
24 ./switch.c
36 ./algorithm.c
17 ./fopen.c
12 ./array.c
22 ./assert.c
20 ./gdb.c
7 ./ascii.c
9 ./index.c
59 ./addmuniu.c
17 ./maths_equal.c
10 ./modle.c
20 ./getchar.c
10 ./gdb2.c
```

最后查看各个 .c 后缀文件的`代码行数`的统计。

find . -name "*.c" -exec cat {} \; | wc -l

```bash
327
```

还可以把各个 .c 后缀文件的源代码合并到一个文件。

find . -name "*.c" -exec cat {} \; > temp.txt


自动分析一个文件的信息及格式 file命令
----------

可以分析出是 ASCII、x86的汇编、ELF的可执行 等文件。

file a.out

```bash
a.out: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 2.6.32, BuildID[sha1]=89a3997654a5ba7a05901db4383d1c08fa2154bb, not stripped
```

ELF的可执行文件，运行在64位的架构上。

查看二进制文件（ELF的可执行文件）里的内容
----------

可看到16进制的数据表示，不再是vim看到的二进制乱码。

如下所示，输出了文件中的每一个字节的`内容`！但看不清整个文件的`结构`。右侧的点号代表是 vim 看到的那些乱码，在此会以`点`来表示。

hexdump -C a.out

```bash
00000000  7f 45 4c 46 02 01 01 00  00 00 00 00 00 00 00 00  |.ELF............|
00000010  02 00 3e 00 01 00 00 00  a0 04 40 00 00 00 00 00  |..>.......@.....|
00000020  40 00 00 00 00 00 00 00  60 1a 00 00 00 00 00 00  |@.......`.......|
00000030  00 00 00 00 40 00 38 00  09 00 40 00 1f 00 1c 00  |....@.8...@.....|
00000040  06 00 00 00 05 00 00 00  40 00 00 00 00 00 00 00  |........@.......|
00000050  40 00 40 00 00 00 00 00  40 00 40 00 00 00 00 00  |@.@.....@.@.....|
00000060  f8 01 00 00 00 00 00 00  f8 01 00 00 00 00 00 00  |................|
00000070  08 00 00 00 00 00 00 00  03 00 00 00 04 00 00 00  |................|
00000080  38 02 00 00 00 00 00 00  38 02 40 00 00 00 00 00  |8.......8.@.....|
00000090  38 02 40 00 00 00 00 00  1c 00 00 00 00 00 00 00  |8.@.............|
000000a0  1c 00 00 00 00 00 00 00  01 00 00 00 00 00 00 00  |................|
000000b0  01 00 00 00 05 00 00 00  00 00 00 00 00 00 00 00  |................|
000000c0  00 00 40 00 00 00 00 00  00 00 40 00 00 00 00 00  |..@.......@.....|
000000d0  a4 0a 00 00 00 00 00 00  a4 0a 00 00 00 00 00 00  |................|
000000e0  00 00 20 00 00 00 00 00  01 00 00 00 06 00 00 00  |.. .............|
000000f0  10 0e 00 00 00 00 00 00  10 0e 60 00 00 00 00 00  |..........`.....|
00000100  10 0e 60 00 00 00 00 00  34 02 00 00 00 00 00 00  |..`.....4.......|
00000110  38 02 00 00 00 00 00 00  00 00 20 00 00 00 00 00  |8......... .....|
00000120  02 00 00 00 06 00 00 00  28 0e 00 00 00 00 00 00  |........(.......|
00000130  28 0e 60 00 00 00 00 00  28 0e 60 00 00 00 00 00  |(.`.....(.`.....|
00000140  d0 01 00 00 00 00 00 00  d0 01 00 00 00 00 00 00  |................|
00000150  08 00 00 00 00 00 00 00  04 00 00 00 04 00 00 00  |................|
00000160  54 02 00 00 00 00 00 00  54 02 40 00 00 00 00 00  |T.......T.@.....|
00000170  54 02 40 00 00 00 00 00  44 00 00 00 00 00 00 00  |T.@.....D.......|
00000180  44 00 00 00 00 00 00 00  04 00 00 00 00 00 00 00  |D...............|
00000190  50 e5 74 64 04 00 00 00  48 09 00 00 00 00 00 00  |P.td....H.......|
000001a0  48 09 40 00 00 00 00 00  48 09 40 00 00 00 00 00  |H.@.....H.@.....|
000001b0  3c 00 00 00 00 00 00 00  3c 00 00 00 00 00 00 00  |<.......<.......|
000001c0  04 00 00 00 00 00 00 00  51 e5 74 64 06 00 00 00  |........Q.td....|
000001d0  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
*
000001f0  00 00 00 00 00 00 00 00  10 00 00 00 00 00 00 00  |................|
00000200  52 e5 74 64 04 00 00 00  10 0e 00 00 00 00 00 00  |R.td............|
00000210  10 0e 60 00 00 00 00 00  10 0e 60 00 00 00 00 00  |..`.......`.....|
00000220  f0 01 00 00 00 00 00 00  f0 01 00 00 00 00 00 00  |................|
00000230  01 00 00 00 00 00 00 00  2f 6c 69 62 36 34 2f 6c  |......../lib64/l|
00000240  64 2d 6c 69 6e 75 78 2d  78 38 36 2d 36 34 2e 73  |d-linux-x86-64.s|
00000250  6f 2e 32 00 04 00 00 00  10 00 00 00 01 00 00 00  |o.2.............|
00000260  47 4e 55 00 00 00 00 00  02 00 00 00 06 00 00 00  |GNU.............|
......
```

查看二进制文件（ELF的可执行文件）的内容`ELF结构信息`
----------

能够查看到整个二进制文件的`结构`和信息。反正是电子版，不怕浪费，就给大家列一个完整的信息如下

readelf -a a.out

```bash
ELF 头：
  Magic：   7f 45 4c 46 02 01 01 00 00 00 00 00 00 00 00 00
  类别:                              ELF64
  数据:                              2 补码，小端序 (little endian)
  版本:                              1 (current)
  OS/ABI:                            UNIX - System V
  ABI 版本:                          0
  类型:                              EXEC (可执行文件)
  系统架构:                          Advanced Micro Devices X86-64
  版本:                              0x1
  入口点地址：               0x400430
  程序头起点：          64 (bytes into file)
  Start of section headers:          6624 (bytes into file)
  标志：             0x0
  本头的大小：       64 (字节)
  程序头大小：       56 (字节)
  Number of program headers:         9
  节头大小：         64 (字节)
  节头数量：         31
  字符串表索引节头： 28

节头：                   //节头表
  [号] 名称              类型             地址              偏移量
       大小              全体大小          旗标   链接   信息   对齐
  [ 0]                   NULL             0000000000000000  00000000
       0000000000000000  0000000000000000           0     0     0
  [ 1] .interp           PROGBITS         0000000000400238  00000238
       000000000000001c  0000000000000000   A       0     0     1
  [ 2] .note.ABI-tag     NOTE             0000000000400254  00000254
       0000000000000020  0000000000000000   A       0     0     4
  [ 3] .note.gnu.build-i NOTE             0000000000400274  00000274
       0000000000000024  0000000000000000   A       0     0     4
  [ 4] .gnu.hash         GNU_HASH         0000000000400298  00000298
       000000000000001c  0000000000000000   A       5     0     8
  [ 5] .dynsym           DYNSYM           00000000004002b8  000002b8
       0000000000000060  0000000000000018   A       6     1     8
  [ 6] .dynstr           STRTAB           0000000000400318  00000318
       000000000000003f  0000000000000000   A       0     0     1
  [ 7] .gnu.version      VERSYM           0000000000400358  00000358
       0000000000000008  0000000000000002   A       5     0     2
  [ 8] .gnu.version_r    VERNEED          0000000000400360  00000360
       0000000000000020  0000000000000000   A       6     1     8
  [ 9] .rela.dyn         RELA             0000000000400380  00000380
       0000000000000018  0000000000000018   A       5     0     8
  [10] .rela.plt         RELA             0000000000400398  00000398
       0000000000000030  0000000000000018  AI       5    24     8
  [11] .init             PROGBITS         00000000004003c8  000003c8
       000000000000001a  0000000000000000  AX       0     0     4
  [12] .plt              PROGBITS         00000000004003f0  000003f0
       0000000000000030  0000000000000010  AX       0     0     16
  [13] .plt.got          PROGBITS         0000000000400420  00000420
       0000000000000008  0000000000000000  AX       0     0     8
  [14] .text             PROGBITS         0000000000400430  00000430
       0000000000000192  0000000000000000  AX       0     0     16
  [15] .fini             PROGBITS         00000000004005c4  000005c4
       0000000000000009  0000000000000000  AX       0     0     4
  [16] .rodata           PROGBITS         00000000004005d0  000005d0
       000000000000000c  0000000000000000   A       0     0     4
  [17] .eh_frame_hdr     PROGBITS         00000000004005dc  000005dc
       0000000000000034  0000000000000000   A       0     0     4
  [18] .eh_frame         PROGBITS         0000000000400610  00000610
       00000000000000f4  0000000000000000   A       0     0     8
  [19] .init_array       INIT_ARRAY       0000000000600e10  00000e10
       0000000000000008  0000000000000000  WA       0     0     8
  [20] .fini_array       FINI_ARRAY       0000000000600e18  00000e18
       0000000000000008  0000000000000000  WA       0     0     8
  [21] .jcr              PROGBITS         0000000000600e20  00000e20
       0000000000000008  0000000000000000  WA       0     0     8
  [22] .dynamic          DYNAMIC          0000000000600e28  00000e28
       00000000000001d0  0000000000000010  WA       6     0     8
  [23] .got              PROGBITS         0000000000600ff8  00000ff8
       0000000000000008  0000000000000008  WA       0     0     8
  [24] .got.plt          PROGBITS         0000000000601000  00001000
       0000000000000028  0000000000000008  WA       0     0     8
  [25] .data             PROGBITS         0000000000601028  00001028
       0000000000000010  0000000000000000  WA       0     0     8
  [26] .bss              NOBITS           0000000000601038  00001038
       0000000000000008  0000000000000000  WA       0     0     1
  [27] .comment          PROGBITS         0000000000000000  00001038
       0000000000000034  0000000000000001  MS       0     0     1
  [28] .shstrtab         STRTAB           0000000000000000  000018cf
       000000000000010c  0000000000000000           0     0     1
  [29] .symtab           SYMTAB           0000000000000000  00001070
       0000000000000648  0000000000000018          30    47     8
  [30] .strtab           STRTAB           0000000000000000  000016b8
       0000000000000217  0000000000000000           0     0     1
Key to Flags:
  W (write), A (alloc), X (execute), M (merge), S (strings), l (large)
  I (info), L (link order), G (group), T (TLS), E (exclude), x (unknown)
  O (extra OS processing required) o (OS specific), p (processor specific)

There are no section groups in this file.

程序头：          //段头表
  Type           Offset             VirtAddr           PhysAddr
                 FileSiz            MemSiz              Flags  Align
  PHDR           0x0000000000000040 0x0000000000400040 0x0000000000400040
                 0x00000000000001f8 0x00000000000001f8  R E    8
  INTERP         0x0000000000000238 0x0000000000400238 0x0000000000400238
                 0x000000000000001c 0x000000000000001c  R      1
      [Requesting program interpreter: /lib64/ld-linux-x86-64.so.2]
  LOAD           0x0000000000000000 0x0000000000400000 0x0000000000400000
                 0x0000000000000704 0x0000000000000704  R E    200000
  LOAD           0x0000000000000e10 0x0000000000600e10 0x0000000000600e10
                 0x0000000000000228 0x0000000000000230  RW     200000
  DYNAMIC        0x0000000000000e28 0x0000000000600e28 0x0000000000600e28
                 0x00000000000001d0 0x00000000000001d0  RW     8
  NOTE           0x0000000000000254 0x0000000000400254 0x0000000000400254
                 0x0000000000000044 0x0000000000000044  R      4
  GNU_EH_FRAME   0x00000000000005dc 0x00000000004005dc 0x00000000004005dc
                 0x0000000000000034 0x0000000000000034  R      4
  GNU_STACK      0x0000000000000000 0x0000000000000000 0x0000000000000000
                 0x0000000000000000 0x0000000000000000  RW     10
  GNU_RELRO      0x0000000000000e10 0x0000000000600e10 0x0000000000600e10
                 0x00000000000001f0 0x00000000000001f0  R      1

 Section to Segment mapping:    //节到段的映射
  段节...
   00
   01     .interp
   02     .interp .note.ABI-tag .note.gnu.build-id .gnu.hash .dynsym .dynstr .gnu.version .gnu.version_r .rela.dyn .rela.plt .init .plt .plt.got .text .fini .rodata .eh_frame_hdr .eh_frame
   03     .init_array .fini_array .jcr .dynamic .got .got.plt .data .bss
   04     .dynamic
   05     .note.ABI-tag .note.gnu.build-id
   06     .eh_frame_hdr
   07
   08     .init_array .fini_array .jcr .dynamic .got

Dynamic section at offset 0xe28 contains 24 entries:    //动态链接信息
  标记        类型                         名称/值
 0x0000000000000001 (NEEDED)             共享库：[libc.so.6]
 0x000000000000000c (INIT)               0x4003c8
 0x000000000000000d (FINI)               0x4005c4
 0x0000000000000019 (INIT_ARRAY)         0x600e10
 0x000000000000001b (INIT_ARRAYSZ)       8 (bytes)
 0x000000000000001a (FINI_ARRAY)         0x600e18
 0x000000000000001c (FINI_ARRAYSZ)       8 (bytes)
 0x000000006ffffef5 (GNU_HASH)           0x400298
 0x0000000000000005 (STRTAB)             0x400318
 0x0000000000000006 (SYMTAB)             0x4002b8
 0x000000000000000a (STRSZ)              63 (bytes)
 0x000000000000000b (SYMENT)             24 (bytes)
 0x0000000000000015 (DEBUG)              0x0
 0x0000000000000003 (PLTGOT)             0x601000
 0x0000000000000002 (PLTRELSZ)           48 (bytes)
 0x0000000000000014 (PLTREL)             RELA
 0x0000000000000017 (JMPREL)             0x400398
 0x0000000000000007 (RELA)               0x400380
 0x0000000000000008 (RELASZ)             24 (bytes)
 0x0000000000000009 (RELAENT)            24 (bytes)
 0x000000006ffffffe (VERNEED)            0x400360
 0x000000006fffffff (VERNEEDNUM)         1
 0x000000006ffffff0 (VERSYM)             0x400358
 0x0000000000000000 (NULL)               0x0

重定位节 '.rela.dyn' 位于偏移量 0x380 含有 1 个条目：
  偏移量          信息           类型           符号值        符号名称 + 加数
000000600ff8  000300000006 R_X86_64_GLOB_DAT 0000000000000000 __gmon_start__ + 0

重定位节 '.rela.plt' 位于偏移量 0x398 含有 2 个条目：
  偏移量          信息           类型           符号值        符号名称 + 加数
000000601018  000100000007 R_X86_64_JUMP_SLO 0000000000000000 printf@GLIBC_2.2.5 + 0
000000601020  000200000007 R_X86_64_JUMP_SLO 0000000000000000 __libc_start_main@GLIBC_2.2.5 + 0

The decoding of unwind sections for machine type Advanced Micro Devices X86-64 is not currently supported.

Symbol table '.dynsym' contains 4 entries:      //符号表
   Num:    Value          Size Type    Bind   Vis      Ndx Name
     0: 0000000000000000     0 NOTYPE  LOCAL  DEFAULT  UND
     1: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND printf@GLIBC_2.2.5 (2)
     2: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND __libc_start_main@GLIBC_2.2.5 (2)
     3: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND __gmon_start__

Symbol table '.symtab' contains 67 entries:
   Num:    Value          Size Type    Bind   Vis      Ndx Name
     0: 0000000000000000     0 NOTYPE  LOCAL  DEFAULT  UND
     1: 0000000000400238     0 SECTION LOCAL  DEFAULT    1
     2: 0000000000400254     0 SECTION LOCAL  DEFAULT    2
     3: 0000000000400274     0 SECTION LOCAL  DEFAULT    3
     4: 0000000000400298     0 SECTION LOCAL  DEFAULT    4
     5: 00000000004002b8     0 SECTION LOCAL  DEFAULT    5
     6: 0000000000400318     0 SECTION LOCAL  DEFAULT    6
     7: 0000000000400358     0 SECTION LOCAL  DEFAULT    7
     8: 0000000000400360     0 SECTION LOCAL  DEFAULT    8
     9: 0000000000400380     0 SECTION LOCAL  DEFAULT    9
    10: 0000000000400398     0 SECTION LOCAL  DEFAULT   10
    11: 00000000004003c8     0 SECTION LOCAL  DEFAULT   11
    12: 00000000004003f0     0 SECTION LOCAL  DEFAULT   12
    13: 0000000000400420     0 SECTION LOCAL  DEFAULT   13
    14: 0000000000400430     0 SECTION LOCAL  DEFAULT   14
    15: 00000000004005c4     0 SECTION LOCAL  DEFAULT   15
    16: 00000000004005d0     0 SECTION LOCAL  DEFAULT   16
    17: 00000000004005dc     0 SECTION LOCAL  DEFAULT   17
    18: 0000000000400610     0 SECTION LOCAL  DEFAULT   18
    19: 0000000000600e10     0 SECTION LOCAL  DEFAULT   19
    20: 0000000000600e18     0 SECTION LOCAL  DEFAULT   20
    21: 0000000000600e20     0 SECTION LOCAL  DEFAULT   21
    22: 0000000000600e28     0 SECTION LOCAL  DEFAULT   22
    23: 0000000000600ff8     0 SECTION LOCAL  DEFAULT   23
    24: 0000000000601000     0 SECTION LOCAL  DEFAULT   24
    25: 0000000000601028     0 SECTION LOCAL  DEFAULT   25
    26: 0000000000601038     0 SECTION LOCAL  DEFAULT   26
    27: 0000000000000000     0 SECTION LOCAL  DEFAULT   27
    28: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS crtstuff.c
    29: 0000000000600e20     0 OBJECT  LOCAL  DEFAULT   21 __JCR_LIST__
    30: 0000000000400460     0 FUNC    LOCAL  DEFAULT   14 deregister_tm_clones
    31: 00000000004004a0     0 FUNC    LOCAL  DEFAULT   14 register_tm_clones
    32: 00000000004004e0     0 FUNC    LOCAL  DEFAULT   14 __do_global_dtors_aux
    33: 0000000000601038     1 OBJECT  LOCAL  DEFAULT   26 completed.7585
    34: 0000000000600e18     0 OBJECT  LOCAL  DEFAULT   20 __do_global_dtors_aux_fin
    35: 0000000000400500     0 FUNC    LOCAL  DEFAULT   14 frame_dummy
    36: 0000000000600e10     0 OBJECT  LOCAL  DEFAULT   19 __frame_dummy_init_array_
    37: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS simple.c
    38: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS crtstuff.c
    39: 0000000000400700     0 OBJECT  LOCAL  DEFAULT   18 __FRAME_END__
    40: 0000000000600e20     0 OBJECT  LOCAL  DEFAULT   21 __JCR_END__
    41: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS
    42: 0000000000600e18     0 NOTYPE  LOCAL  DEFAULT   19 __init_array_end
    43: 0000000000600e28     0 OBJECT  LOCAL  DEFAULT   22 _DYNAMIC
    44: 0000000000600e10     0 NOTYPE  LOCAL  DEFAULT   19 __init_array_start
    45: 00000000004005dc     0 NOTYPE  LOCAL  DEFAULT   17 __GNU_EH_FRAME_HDR
    46: 0000000000601000     0 OBJECT  LOCAL  DEFAULT   24 _GLOBAL_OFFSET_TABLE_
    47: 00000000004005c0     2 FUNC    GLOBAL DEFAULT   14 __libc_csu_fini
    48: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND _ITM_deregisterTMCloneTab
    49: 0000000000601028     0 NOTYPE  WEAK   DEFAULT   25 data_start
    50: 0000000000601038     0 NOTYPE  GLOBAL DEFAULT   25 _edata
    51: 00000000004005c4     0 FUNC    GLOBAL DEFAULT   15 _fini
    52: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND printf@@GLIBC_2.2.5
    53: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND __libc_start_main@@GLIBC_
    54: 0000000000601028     0 NOTYPE  GLOBAL DEFAULT   25 __data_start
    55: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND __gmon_start__
    56: 0000000000601030     0 OBJECT  GLOBAL HIDDEN    25 __dso_handle
    57: 00000000004005d0     4 OBJECT  GLOBAL DEFAULT   16 _IO_stdin_used
    58: 0000000000400550   101 FUNC    GLOBAL DEFAULT   14 __libc_csu_init
    59: 0000000000601040     0 NOTYPE  GLOBAL DEFAULT   26 _end
    60: 0000000000400430    42 FUNC    GLOBAL DEFAULT   14 _start
    61: 0000000000601038     0 NOTYPE  GLOBAL DEFAULT   26 __bss_start
    62: 0000000000400526    40 FUNC    GLOBAL DEFAULT   14 main
    63: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND _Jv_RegisterClasses
    64: 0000000000601038     0 OBJECT  GLOBAL HIDDEN    25 __TMC_END__
    65: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND _ITM_registerTMCloneTable
    66: 00000000004003c8     0 FUNC    GLOBAL DEFAULT   11 _init

Version symbols section '.gnu.version' contains 4 entries:
 地址： 0000000000400358  Offset: 0x000358  Link: 5 (.dynsym)
  000:   0 (*本地*)       2 (GLIBC_2.2.5)   2 (GLIBC_2.2.5)   0 (*本地*)

Version needs section '.gnu.version_r' contains 1 entries:
 地址：0x0000000000400360  Offset: 0x000360  Link: 6 (.dynstr)
  000000: 版本: 1  文件：libc.so.6  计数：1
  0x0010：名称：GLIBC_2.2.5  标志：无  版本：2

Displaying notes found at file offset 0x00000254 with length 0x00000020:
  Owner                 Data size	Description
  GNU                  0x00000010	NT_GNU_ABI_TAG (ABI version tag)
    OS: Linux, ABI: 2.6.32

Displaying notes found at file offset 0x00000274 with length 0x00000024:
  Owner                 Data size	Description
  GNU                  0x00000014	NT_GNU_BUILD_ID (unique build ID bitstring)
    Build ID: 3c464d52c1ffcb6a1a64bbaca759ff13226ecfca
```
