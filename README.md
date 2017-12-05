[TOC]

计算机原理核心基础
============================

**作者：克里斯王(王亮)  
天狮集团 PHP技术经理 工号019213**

这本书要开源吗? 还没想清楚~ 先写完再说。


# 写书平台 #

Ubuntu + Github Markdown


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
    
  - 循环
    
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
    
    ```
    sudo aptitude install build-essential
    ```
    
  - 编辑 index.c
  
    `````
    #include <stdio.h>

    /* main: the output */
    int main(void)
    {
            printf("Hello,My Baby.\n");
            return 0;
    }
    `````
    
  - 编译
  
    ```
    sudo gcc -Wall index.c
    ./a.out         <--------- a.out 是 Assembler Output 缩写，意思是汇编输出。
    ```
    
    输出
    
    Hello,My Baby.
    
  - 看一下经过**汇编**，再被汇编器翻译成**机器语言**的文件：
    
    ```
    sudo vim a.out            // 也可以自行安装 hexedit 来查看
    :%!xxd -g 1
    ```
    
    ```
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

    -字符型
    
      `char`
      
    -整型
    
      `int`
      
    -单精度浮点型
    
      `float`
      
    -双精度浮点型 等
    
      `double`
      
  - **声明**在C语言中的定义有3种：

    - 变量声明
    
    - 函数声明
    
    - 类型声明
    
    
