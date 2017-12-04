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


C编程
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
    sudo gcc index.c
    ./a.out
    ```
