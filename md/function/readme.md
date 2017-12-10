[返回目录](/README.md)

函数
===========================


内置函数的调用(如 log())
----------

数学函数会使用到C标准库：
  
  - 例子
    
    ```c
    #include <stdio.h>
    #include <math.h>       // 引入C标准库的 math

    int main(void)
    {
            double pi = 3.1416;
            
            printf("Sin(pi/2) = %f\nLn(1) = %f\n", sin(pi/2), log(1.0));
            return 0;
    }
    ```
    
    `sudo gcc -Wall maths.c -lm`        //**链接**上外部库math.h（通常位于 /usr/include/）
    
    GCC 的默认选项本身就有 -lc，此处所以使用 `-lm`，是因为 math 头文件存在于库文件 `libm` 里（简写为lm，即`-lm`）。
      
      注意观察：
        
        printf 来自 libc 库文件，因为 GCC 默认就是用了 -lc 参数，所以默认就包含了词库文件！此库文件支持的是 stdio 头文件的 printf！！
    
    ```bash
    Sin(pi/2) = 1.000000
    Ln(1) = 0.000000
    ```
    
  - 数学函数，或者说函数调用(Function Call)，也是一种**表达式**。
  
  
函数的返回
----------

返回给谁？C语言返回给的自然是OS操作系统。

  - 使用 bash 的特殊变量 $? 来获取C语言返回的数据，它是表示上一条命令的退出状态。
    
    `````C
    int main(void)
    {
      int year = 2050;
      printf("the year %d", year);
      return 26;
    `````
    
    ```bash
    ./a.out
    the year is 2050
    
    echo $?
    26
    ```

函数的声明和定义顺序
----------

以**先声明后调用**的原则，被调用的函数，必须先被声明。

  - 声明顺序必须这样
    
    `````c
    #include <stdio.h>
    
    /* 孙子 */
    void grandson(void)
    {
      ...
    }
    
    /* 儿子 */
    void son(void)
    {
      grandson();
    }
    
    /* 老子 */
    int main(void)
    {
      son();
      return 0;
    }
    `````
  
  - 如果想摆脱编译器的调用原则，可以使用不带函数体的纯声明，该使定义函数体的顺序随意！：
    
    `````c
    #include <stdio.h>
    
    /* 无函数体的纯声明，此处已经有了顺序，所以下边函数体的定义就无需再遵循编译器的顺序了。其实就是顺序在此处已定！ */
    void grandson(void);      // 完整的声明，就是 函数原型
    void son();       /* 不完整的声明（缺少指明参数类型及个数），则不是 函数原型；
                        【此处编译器不会检查参数类型及个数，至把关不严，编译器会直接通过】；
                        会导致编译器使用 隐式声明，即默认给 参数类型定义为 void，大概了解即可
                      */
    
    /* 老子 */
    int main(void)
    {
      son();          // 致编译器使用 隐式声明，即默认给 参数类型定义为 void，大概了解即可
      return 0;
    }
    
    /* 孙子 */
    void grandson(void)
    {
      ...
    }
    
    /* 儿子 */
    void son(void)
    {
      grandson();
    }
    `````
