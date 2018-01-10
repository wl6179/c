[返回目录](/README.md)

解锁技能 —— **打印**一切
============================

打印`整数`的十六进制值
----------

```c
#include <stdio.h>

int main(void)
{
        int num = 10;

        printf("the num is: %x\n", num);

        return 0;
}

输出（十六进制的 num变量）
the num is: a
```

打印`整数`的**地址**的十六进制值
----------

```c
#include <stdio.h>

int main(void)
{
        int num = 10;

        printf("the num is: %p\n", &num);

        return 0;
}

输出（十六进制的 num变量的地址）
the num is: 0x7ffcfc6c3eb4
```

打印`函数`的十六进制值（地址）
----------

```c
#include <stdio.h>

int main(void)
{
	int num = 10;

	printf("the main()'s address is: %p\n", main);

	return 0;
}

输出（十六进制的 main函数的地址）
the main()'s address is: 0x400596
```
