[返回目录](/README.md)

库
===========================

查看可执行文件`依赖于`哪些共享库：

```bash
ldd a.out
```

`反汇编`查看：

```bash
objdump -dS abc.o
输出省略
```

查看编译器查找目录：

```bash
gcc -print-search-dirs
```

静态库
----------

怎么创建一个库，可在不同程序中使用。（里边有函数和变量可用）

- 代码：

```c
/* stack.c */
char stack[512];
int top = -1;
```

```c
/* push.c */
extern char stack[512];
extern int top;

void push(char c)
{
	stack[++top] = c;
}
```

```c
/* pop.c */
extern char stack[512];
extern int top;

char pop(void)
{
	return stack[top--];
}
```

```c
/* is_empty.c */
extern int top;

int is_empty(void)
{
	return top == -1;
}
```

```c
/* stack.h */
#ifndef STACK_H
#define STACK_H
extern void push(char);
extern char pop(void);
extern int is_empty(void);
#endif
```

```c
/* main.c */
#include <stdio.h>
#include "stack.h"

int main(void)
{
	push('a');
	return 0;
}
```


- `-l 选项`指定的**库**：

  比如 -lstack，

  编译器会首先找有没有`共享库` **lib**stack.so，如果有就链接它；

  如果没有就找有没有`静态库` **lib**stack.a，如果有就链接它；

  ```
  所以`编译器`是优先考虑`共享库`的，如果希望编译器只链接静态库，可以指定 `-static 选项`。
  ```

- 把 *.c 编译成`目标文件` *.o：

  ```bash
  gcc -c stack/stack.c stack/push.c stack/pop.c stack/is_empty.c    # 生成了 stack.o push.o pop.o is_empty.o
  ```

- 打包成静态库 libstack.a：

  库文件名都是以 lib 开头命名，静态库以 .a 后缀。 s 表示为`静态库`创建`索引`，这个索引被`链接器`使用。

  ```bash
  ar rs libstack.a  stack.o push.o pop.o is_empty.o     # 创建静态库，并为静态库创建 索引。
  ```

  或者可以这样分开操作：

  ```bash
  ar r libstack.a stack.o push.o pop.o is_empty.o     # 创建静态库
  ranlib libstack.a       # 可以单独为静态库创建 索引
  ```

- 把 libstack.a 和 main.c `编译` `链接`在一起：

  ```bash
  gcc main.c -L. -lstack -Istack -o main      # -L.在当前目录找，-lstack 就是 libstack.a！
  ```

动态库
----------
