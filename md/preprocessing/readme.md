[返回目录](/README.md)

预处理
===========================

宏定义
----------

- 变量式`宏`定义

  ```c
  #define X 3
  #undef X      // 先取消宏定义
  #define X 2   // 重新宏定义
  ```

- 函数式`宏`定义

  - 函数式宏定义的参数没有类型，预处理器只负责做形式上的`替换代码`，而不做参数类型检查；（格外小心）

  - 函数式宏定义，会在调用代码处替换整个函数代码，所以它代码比较多`文件很大`；

  ```c
  #define device_init_wakeup(dev,val) \
          do { \
                  device_can_wakeup(dev) = !!(val); \     // 必须要用 while(0) 包括起来，否则会被此处的分号;结束掉整个 函数式宏定义！
                  device_set_wakeup_enable(dev,val); \
          } while(0)
  ```

预处理用法：
----------

- `Header Guard 的预处理` 的用法：

  ```c
  #ifndef XXX
  #define XXX
  /* body of XXX */
  #endif    /* XXX */
  ```

  注意：

    #if !defined x，相当于 #ifndef x



- `条件预处理`指示 的用法：

  ```c
  #if VERSION == 1
    int yii;
  #elif VERSION == 2
    long yii;
  #else
    #error UNKNOWN TARGET VERSION
  #endif
  ```

  - 如上例子，`#error` 预处理指示：

    如上例子，编译器遇到这个 `#error 预处理指示`，就报错退出。#error 这也是一个预处理指示！错误信息就是 `UNKNOWN TARGET MACHINE`。

  - 如上例子，如何给 `VERSION` 定义一个值：

    ```bash
    给每个.h都改一下值为2，这是最糟的一种方法：（最差）
    #define VERSION 2
    放弃此方式。

    还可以在`编译`时，给 `VERSION` 定义一个值，写成类似这样的命令：（较好）
    gcc -c -DVERSION=2 main.c       # 编译2.0版本的程序
    ```

    - 还有另一种给 `VERSION` 定义一个值的方法：（最佳）

      那就是设置 Makefile。下一章再说。

何为`展开`：
----------

用一个例子来直接说明，什么叫预处理的（代码的）`展开`。

- 预处理语句，是如何被`编译时`替换代码的：

  假设这段预处理代码

  ```c
  #define VERSION 2
  #if defined x || y || VERSION < 3         // 如果 x 这个`宏`有定义
  ```

  编译后如下被替换：

  ```c
  #define VERSION  2
  #if 0 || 0 || 2 < 3             // 这就是`编译`过程！（预告一下）
  ```
    注意：

      注意，即使前面定义了一个`变量`名是 y（并非宏），呢么在这一步 y 也还是替换成 0。因为 #if 的表达式必须在`编译时`求值，其中包含的名字只能是`宏定义`才能影响之。

  猜一猜，最终会不会被编译替换成这样：

    ```c
    #if 1
    ```

- C标准规定了几个特殊的宏：

  __FILE__、__LINE__ 等。

  可以展开为当前源文件的文件名，是一个`字符串`；或当前代码行的行号，是一个`整数`；

  这两个宏在源代码中不同的位置使用会`自动取不同的值`，显然不是用 #define 能定义得出来的。它们是`编译器内建的特殊的宏`。

  例如

    assert 头文件库中，就使用到了这两个特殊的宏，可查看`函数`部分内容。


- 综合示例：

  咱们自己实现一个 assert 函数，用宏定义。如下

  ```c
  /* assert.h 宏定义 */
  #undef assert	/* 去掉默认存在的 assert 定义 */

  #ifdef NDEBUG
  	#define assert(test)	((void)0)
  #else		/* NDEBUG not defined */
  	void _Assert(char *);        // 在另一个文件中定义，声明一下
  	/* 宏指令 */
  	#define _STR(x) _VAL(x)
  	#define _VAL(x) #x           // 将整型转换成`字符串`#x
  	#define assert(test)	((test) ? (void)0 : _Assert(__FILE__ ":" _STR(__LINE__) " " #test))
  #endif
  ```

  ```c
  /* assert.c */
  #include <stdio.h>
  #include <stdlib.h>

  void _Assert(char *mesg)
  {		/* 打印信息并中断 */
  	fputs(mesg, stderr);         // stderr 标准错误
  	fputs(" -- assertion failed\n", stderr);

  	abort();                     // 中断当前进程
  }
  ```

  ```c
  /* main.c */
  #include "assert.h"       // 用户自定义的 非标准`头文件`

  int main(void)
  {
  	assert(0 > 1);

  	return 0;
  }
  ```

  编译运行：

  ```bash
  gcc -Wall main.c xassert.c -o assert
  ./assert

    main.c:6 0>1 -- assertion failed
    Aborted
  ```
