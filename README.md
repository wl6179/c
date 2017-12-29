计算机原理核心基础（C语言）
============================

**作者：克里斯王(王亮)  
[天狮集团](http://www.sohu.com/a/209730660_461398 '中央级媒体组团近观天狮_搜狐健康_搜狐网') PHP技术经理 工号019213**

这本书要开源吗? 还没想清楚~ 先写完再说。

至少先让天狮集团的PHP团队的小伙子们，能看上这么一本快速上升的资料吧。如果反响好给赞赏，就证明此书的出版有意义，便会最终永久免费开放给大家。书中只顾及实战和原理讲解，小伙伴都是辛劳的码夫，唯一需求就是立刻上手，深入浅出。

这是一本了解编程底层的高级入门书，看完这本书，别再告诉我你学不会C语言了。没错，这本书专治顽疾，各种学不会C语言的顽疾。为此，我已请出八大天神为你加油！


# 写书平台 #

Ubuntu + Atom + Github + Markdown


## 目录（天龙八部） ##

- 第一部 帝释天，降龙

  [第1章 C标准库和glibc](md/glibc/readme.md)

  [第2章 函数](md/function/readme.md)

  [第3章 Base基本语法](md/basicsyntax/readme.md)

  [第4章 遇到算法](md/algorithm/readme.md)

- 第二部 龙王，六脉

  [第5章 结构](md/structure/readme.md)

  [第6章 数组](md/array/readme.md)

  [第7章 字符串](md/string/readme.md)

  [第8章 数字类型](md/type/readme.md)

  [第9章 调试工具](md/testing/readme.md)

- 第三部 夜叉，无相

  [第10章 二进制 逻辑门电路表示](md/onezero/readme.md)

  [第11章 运算符 之 二进制处理数学方法](md/operator/readme.md)

  [第12章 计算机原理 二进制眼光](md/architecture/readme.md)

- 第四部 乾达婆，百变

  [第14章 链接](md/link/readme.md)

  [第15章 定义声明](md/declare/readme.md)

  [第16章 库](md/library/readme.md)

  [第17章 预处理](#)

  [第18章 编译](#)

- 第五部 阿修罗，星移

  [第19章 指针](md/pointer/readme.md)

- 第六部 迦楼罗，易筋

  [第20章 标准库](md/stl/readme.md)

- 第七部 紧那罗，般若

  [第21章 Linux内核](md/linuxkernel/readme.md)

- 第八部 摩呼罗迦，逍遥

  [第22章 PHP扩展](md/phpextend/readme.md)

[TOC]


开场 Cookie
============================

程序的构成
----------

由一堆 **指令** 构成：

  - 输入

    `从键盘设备、文件、或其他设备获取数据`

  - 输出

    `输出到屏幕、写入一个文件、发送到其他设备等`

  - 基本运算

    `加减乘除、数据存取`

  - 测试和分支

    `测试条件，根据不同测试结果，执行不同后续指令`

  - 循环（与[递归](md/algorithm/readme.md)等同）

    `重复执行操作`

就是以上这么几条简单的指令类型，构成了复杂的程序。程序员需要把程序，最终分解成指令。

**低级语言：**

  ~~~
  机器语言和汇编语言，直接用计算机指令编写程序。

  汇编语言：
  就是简单的做一个替换工作，机器语言有几条，汇编也是有几条指令一一对应。

  ~~~

**高级语言：**

  ~~~
  用语句编写程序，而语句是计算机指令的抽象表示。

  高级语言：
  一条语句，将不会是简简单单的一一对应的翻译关系。而是一条语句对应多条汇编或机器指令！这个转化过程就是**编译**。可见编译比汇编器原理要复杂得多，而高级程序必须经过编译器编程机器指令后，才可以执行。
  可是高级语言也有一个天生的优点，就是与平台无关性。编译器将解决一切可移植问题。
  ~~~


完整的开始C编程
----------

运行程序：

  - 安装*GNU C*环境

    ```bash
    sudo aptitude install build-essential
    ```

  - 编辑 index.c

    `````c
    #include <stdio.h>

    /* main: the output */
    int main(void)
    {
            printf("Hello,My Baby.\n");
            return 0;
    }
    `````

  - 编译

    ```bash
    sudo gcc -Wall index.c      // -Wall 甚至能检测出一定的 逻辑错误！
    ./a.out         <--------- a.out 是 Assembler Output 缩写，意思是汇编输出。
    ```

    输出

    Hello,My Baby.

  - 看一下经过**汇编**，再被汇编器翻译成**机器语言**的文件：

    ```vim
    sudo vim a.out            // 也可以自行安装 hexedit 来查看
    :%!xxd -g 1
    ```

    ```vim
    00000220: f0 01 00 00 00 00 00 00 f0 01 00 00 00 00 00 00  ................
    00000230: 01 00 00 00 00 00 00 00 2f 6c 69 62 36 34 2f 6c  ......../lib64/l
    00000240: 64 2d 6c 69 6e 75 78 2d 78 38 36 2d 36 34 2e 73  d-linux-x86-64.s
    00000250: 6f 2e 32 00 04 00 00 00 10 00 00 00 01 00 00 00  o.2.............
    00000260: 47 4e 55 00 00 00 00 00 02 00 00 00 06 00 00 00  GNU.............
    00000270: 20 00 00 00 04 00 00 00 14 00 00 00 03 00 00 00   ...............
    00000280: 47 4e 55 00 27 cd 0d 87 3c 88 cc 7d f1 50 80 7b  GNU.'...<..}.P.{
    00000290: 8a d4 3a d5 6e da 7a 22 01 00 00 00 01 00 00 00  ..:.n.z"........
    000002a0: 01 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  ................
    000002b0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  ................
    000002c0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  ................
    000002d0: 0b 00 00 00 12 00 00 00 00 00 00 00 00 00 00 00  ................
    000002e0: 00 00 00 00 00 00 00 00 10 00 00 00 12 00 00 00  ................
    000002f0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  ................
    00000300: 22 00 00 00 20 00 00 00 00 00 00 00 00 00 00 00  "... ...........
    00000310: 00 00 00 00 00 00 00 00 00 6c 69 62 63 2e 73 6f  .........libc.so
    00000320: 2e 36 00 70 75 74 73 00 5f 5f 6c 69 62 63 5f 73  .6.puts.__libc_s
    00000330: 74 61 72 74 5f 6d 61 69 6e 00 5f 5f 67 6d 6f 6e  tart_main.__gmon
    00000340: 5f 73 74 61 72 74 5f 5f 00 47 4c 49 42 43 5f 32  _start__.GLIBC_2
    00000350: 2e 32 2e 35 00 00 00 00 02 00 02 00 00 00 00 00  .2.5............
    ```

理解字符
----------

*转义序列* 与 *占位符* 的区别

  - 转义字符：

    - 在编译时就直接处理

  - 占位符：

    - 在运行时才处理，例如由 printf **解释** 成转换说明


变量
----------

代表存储器的一块空间，随时可改变。

  - 也有不同的类型：（使用*声明*来控制）

    - 字符型 `char`

    - 整型 `int`

    - 单精度浮点型 `float`

    - 双精度浮点型 `double` 等

  - **声明**在C语言中的有3种：

    - 变量声明

    - 函数声明

    - 类型声明

    或者另外一个角度来说有2种：

    - 也是定义的声明 `分配了存储空间的（注意 是在[编译器]期间就根据 变量声明 来决定是否分配空间）`

      ```同理，函数声明带函数体的，就是等于要求编译器为其生成指令，也就需要存储空间来存储这些要生成的指令，那么，这个函数声明也是函数定义```

    - 不是定义的声明 `未分配了存储空间的声明，就不同时是定义了`

      ```同理，那些不带函数体定义的函数声明，就不是函数定义了```

      ```同理2，类型声明都不带定义，即总不分配存储空间，所以类型声明本身只有声明，不是定义```


对**编码**的初步理解
----------

最常用的是 ASCII 码。从此处 ASCII 的对应关系可以看出，char型～本质上与int型一样都是整数！！只不过取值范围只有 0～127 。所以 int & char 类型完全可以统称为：**整型 Integer Type**。

  字符用**数字**表示的例子

    'a' + 25 与 'z' 的值，是相等的。
    '0' + 9 与 '9' 的值，也相等。
    但注意：
    '0' 和整数值 0 是不相等的。

  字符用**转义**表示的例子

    八进制：
    '\八进制'   例如 '\11' 也表示 Tab制表符;  '\0' 表示 null空字符。

    十六进制：
    '\x十六'    例如 '\x9' 表示 Tab制表符。

    注意:
    '0' 的ascii码是 48, 而不是 0;
    '\0'的ascii码是 0                    <----------- 注意是转义字符, 在发挥作用!



欣赏二维码：

![赞赏作者：克里斯王](ig/wx001.png)

My Github: http://github.com/wl6179/c/
