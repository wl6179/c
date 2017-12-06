函数
===========================


数学函数
----------

数学函数会使用到C标准库：
  
  - 例子
    
    ```
    #include <stdio.h>
    #include <math.h>       //引入C标准库的 math

    int main()
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
    
    ```
    Sin(pi/2) = 1.000000
    Ln(1) = 0.000000
    ```
    
  - 数学函数，或者说函数调用（Function Call），也是一种**表达式**。
