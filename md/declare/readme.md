[返回目录](/README.md)

定义声明
===========================



extern 声明`函数原型`
----------

extern 英语解释是`外部的`。

即表示函数的定义在别的文件中。

```c
/* main.c */
#include <stdio.h>

void push(char);
char pop(void);
int is_empty(void);
extern int top;     // 变量top在另一个.c文件中定义（前提是它没有使用static关键字时，此处才可用extern声明外部定义！ 链接）！即 变量top 具有 External Linkage

int main(void)
{
	push('a');
	push('b');
	push('c');
	printf("%d\n", top);
  ...
}
```

static
----------

用`static关键字`把某些变量，声明为 Internal Linkage 的变量，即其它的.c文件将无法访问当前.c文件中函数定义的变量！（C语言链接多个.c时，是以.c文件的不同，来划分不同作用域的）

```c
/* stack.c */
static char stack[512];
static int top = -1;      // 如果此处（stack.c中）使用了static关键字，则 top变量 具有 Internal Linkage，别处将无法引用！ 链接
                          // 从而保护了stack.c模块的内部状态，这也是一种封装！
void push(char c)
{
	stack[++top] = c;
}

...
```

**总结 static 作用：**

在一个模块中，有些函数是提供给外界使用的，这些函数可以声明为 External Linkage 的;

相反，有些函数只在模块内部使用，而不希望被外界访问到，则声明为 Internal Linkage 的。（用 static）

**例如可以这么说：**

stack.c 这个模块（.c文件），`封装了` top 和 stack 两个变量; (用 static)

并且`导出了` push、pop、is_empty 三个函数接口。

头文件
--------

从上边多个.c文件编译，可以看出：一个公共的.c函数文件，在多个地方需要`调用`的时候，都需要去做一次 `extern 声明`的代码！这会造成大量冗余代码。

- 自己写一个`.h头文件`

  ```c
  /* stack.h */
  #ifndef STACK_H       // 如果STACK_H这个`宏`没有定义过，则在预处理中输出。（这种保护头文件的写法，避免了头文件的内容被`重复包含`）
  #define STACK_H
  // 外部声明：
  extern void push(char);
  extern char pop(void);
  extern int is_empty(void);
  #endif                // 如果STACK_H这个`宏`没有定义过，则在预处理中输出。
  ```

  如上，`宏定义名`就用头文件名的`大写`形式，这是规范。

  ```c
  /* stack.c */
  static char stack[512];
  static int top = -1;

  // 函数接口定义
  ...
  ```

  ```c
  /* main.c */
  #include <stdio.h>
  #include "stack.h"      // 不用尖括号的头文件引用，会先查找-I指定的目录; 然后才和尖括号的一样。

  int main(void)
  {
    // 调用函数接口
  	...
  }
  ```

- 注意：

  不用尖括号的头文件引用，会先查找`-I`指定的目录; 然后才和尖括号的一样。

  例如

  ```bash
  gcc -c main.c -Isubfolder           # 编译时，注明头文件的首查找目录为：subfolder/
  ```

  或者也可以写全路径，如

  `把上面的代码改成 #include "subfolder/stack.h"  ，那么编译时就不需要加 -Isubfolder 选项了`

  ```c
  /* main.c */
  #include <stdio.h>
  #include "subfolder/stack.h"    // 这里

  int main(void)
  {
    // 调用函数接口
  	...
  }
  ```

- 为什么要包含头文件.h，而不是.c文件

  避免了多次包含同一个.c文件，导致同时多次定义同样的变量和函数。报错！

- 头文件中可以`声明`变量、函数！但一定不能出现它们的`定义`。

  即便是头文件，也不可定义，否则还是会出现和多次包含同一个.c文件的问题，而导致同时多次定义同样的变量和函数。报错！
