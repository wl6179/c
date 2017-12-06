函数
===========================


数学函数
----------

会使用到C标准库：
  
  - 例子
    
    ```
    #include <stdio.h>
    #include <math.h>

    int main()
    {
            double pi = 3.1416;

            printf("Sin(pi/2)=%f\nLn1=%f\n", sin(pi/2), log(1.0));
            return 0;
    }
    ```
    
    `sudo gcc -Wall maths.c -lm`        //**链接**上外部库math.h
    
    ```
    Sin(pi/2)=1.000000
    ln1=0.000000
    ```
    
