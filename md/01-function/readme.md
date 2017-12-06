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
    
    `sudo gcc -Wall maths.c -lm`        //**链接**上外部库math.h
    
    ```
    Sin(pi/2) = 1.000000
    Ln(1) = 0.000000
    ```
    
  - 数学函数，或者说函数调用 Function Call，也是一种**表达式**。
