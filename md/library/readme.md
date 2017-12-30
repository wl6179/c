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
````

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

  这种指定库的方式`编译`（非挨个.c文件的一块编译），可以确保生成代码的`最优化`，多余的、未被调用的函数代码，不会被加入最终可执行文件的代码中。

动态库
----------

- 代码：

  同上。静态库的代码示例。

- `-fPIC 选项` 生成位置无关的代码：

  共享库各段的加载地址并没有定死，可以加载到任意位置。因为`指令`中没有使用绝对地址，因此称为位置无关代码。

- 把 *.c 编译成`位置无关的目标文件` *.o：

  ```bash
  gcc -c -fPIC stack/stack.c stack/push.c stack/pop.c stack/is_empty.c
  ```

- 生成`共享库` libstack.so：

  ```bash
  gcc -shared -o  libstack.so  stack.o push.o pop.o is_empty.o
  ```

- 把 main.c 和共享库`编译` `链接`在一起：

  共享库的搜索路径，由`动态链接器`决定

    1.首先在环境变量 LD_LIBRARY_PATH 所记录的路径中查找；

    2.然后从缓存文件 /etc/ld.so.cache 中查找。这个缓存文件由 ldconfig 命令读取配置文件 /etc/ld.so.conf 之后生成；

    3.如果上述步骤都找不到，则到默认的系统路径中查找，先是 /usr/lib，然后是 /lib；

  ```bash
  gcc main.c -g -L. -lstack -Istack -o main     # -g
  ```
