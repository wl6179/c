[返回目录](/README.md)

第五部 阿修罗，星移

![第五部 阿修罗，星移](/ig/5.jpg)


指针
===========================

概念
----------

- `指针类型`的变量：

  只存`地址`。

  64位系统时的指针类型，占8个字节；

  32位系统时的指针类型，占4个字节；

- 指针类型做`全局变量`时

  因为普通变量 word 的地址，是在编译链接时就能确定的，而不需要到`运行时`才知道。所以 &word 是`常量表达式`。

  如

  ```c
  // 全局（char *型 的指针类型）
  char *pword = &word;        // 是没有问题的。因为编译时 &word 已是常量（地址）。
  /* 以上全是 定义声明 */
  ```

- `指针类型`，再`加上 *号`来表达式，则是一个`左值`！：

  例如

  ```c
  int j;
  int *pi = &j;
  /* 以上全是 定义声明 */

  /* 以下全是 运算！： */
  *pi = *pi + 111;  // 即 j + 111（指针类型 加*号 来运算时，可做`左值`！）

  int *pai;     // 新定义一个指针类型（或 int *pai = pi; 也一样，注意*星号的用法）
  pai = pi;     // 注意，pi 没有加*号 来运算！！所以pi依然是一个`指针类型`，并`非左值`！
  ```

- 确保对指针类型的变量，都要初始化：

  至少赋值一个 NULL（这就是`空指针`）。否则就会成为`野指针`！大概率的导致各种难以察觉的`数据`错误。

  ```c
  #include <stddef.h>

  int main(void)
  {
  	int *p = NULL;   // （这就是`空指针`）OS会确保不会在 NULL 即地址0 的附近，保存任何数据！
  	...
  	*p = 0;
  	...
  }
  ```

  - NULL（这就是`空指针`），在C标准库的头文件 stddef.h 中定义：

    ```c
    #define NULL ((void *)0)          // void *类型，是C标准中规定的`通用指针`，用于各种指针类型转换。
    ```

    void *指针，与其它类型的指针之间可以`隐式转换`，而不必用`类型转换运算符`()。

    举个例子：

      ```c
      void func(void *pv)     // void * 此处就可对 各指针类型 进行 隐式转换
      {
      	/* *pv = 'A' is illegal */
      	char *pchar = pv;
      	*pchar = 'A';
      }

      int main(void)
      {
      	char c;
      	func(&c);

      	printf("%c\n", c);
        ...
      }
      ```

斗转星移 - 跨域访问
----------

通过`地址`（指针的特点），可以去改变`非自己作用域`的变量。

```c
#include <stdio.h>

int *swap(int *px, int *py)
{
	int temp;
	temp = *px;
	*px = *py;
	*py = temp;  // 交换 *px 和 *py 的值！!（就是直接操作 px he py 地址，即 外部的实参：&i, &j。）
	return px;
}

int main(void)
{
	int i = 10, j = 20;        // 此处main函数的i、j，将被`swap函数`改变！
	int *p = swap(&i, &j);
	printf("now i=%d j=%d *p=%d\n", i, j, *p);
	return 0;
}
```

数组与指针
----------

```c
int a[10];
int *pa = &a[0];
pa++;     // pa是 int *类型指针，所以 pa++，就是 pa地址+ 4（个字节）！
```

指针与数组，基本是等价的，都可以用`下标`来访问。所以，可以有如下的简写：

```c
int a[10];      // 整型数组

int *pa = &a[0];
等价于：
int *pa = a;    // 整型指针（都可以用`下标`来访问）。数组名做右值时转换成指向`首元素的指针`。但做左值仍然表示`整个数组的存储空间`，而不是首元素的存储空间。
```

和数组一样，下标为负数是可以的。如指针类型 `pa[-1]` 是合法的，它等于 `a[0]` 是同一个元素。

因此，指针类型在做加减运算时，只有俩指针指向同一个数组中元素的指针之间，相互比较才有意义，否则没有意义。

- 在函数中，参数是数组，等价于参数是指针：

```c
void func(int a[10])    // 数组（ 还可以等价于 void func(int a[]) ）
{
	...
}
```

等价于：

```c
void func(int *a)       // 指针 int *
{
	...
}
```

写成指针形式还是数组形式对编译器来说没区别，都表示这个参数是`指针`！

- 指针数组：

  每个元素都是`int *指针`.

  ```c
  int *a[10];
  ```

- `main函数`的标准原型

  是 int main(int argc, char *argv[]);。argc是命令行参数的个数；而argv是一个`指向指针的指针`；为什么不是指针数组呢？因为函数原型中的[]，表示`指针`而不表示`数组`，`等价于char **argv`。

    那为什么要写成 `char *argv[]`，而不写成 `char **argv` 呢？

      这样写给读代码的人提供了有用信息，argv 不是指向单个指针，而是指向一个`指针数组的`首元素。

  例子：

    ```c
    /*
     * 注：完整的 main 函数，可以根据执行程序的入口参数做更精细的控制
    */
    int main(int argc, char *argv[])
    {
    	int i;

    	for(i = 0; i < argc; i++)
    		printf("argv[%d]=%s\n", i, argv[i]);

    	return 0;
    }
    ```

  输出：

    ```bash
    ./a.out a b c

    argv[0]=./a.out
    argv[1]=a
    argv[2]=b
    argv[3]=c


    ln -s a.out printargv
    ./printargv d e

    argv[0]=./printargv
    argv[1]=d
    argv[2]=e
    ```

    NULL 标识着 argv 的结尾！因而也可以这么写，碰到 NULL 结束：

      ```c
      for(i=0; argv[i] != NULL; i++)
      {
        ...
      }
      ```

自定义结构体与指针
----------

```c
struct unit {
	char c;
	int num;
};
struct unit u;
struct unit *p = &u;

int main(void)
{
  u.c = 1;
  u.num = 2;

  printf("%d %d %d %d\n", (*p).c, (*p).num, p->c, p->num);
  return 0;
}
```
