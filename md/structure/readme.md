[返回目录](/README.md)

第二部 龙王，六脉

![第二部 龙王，六脉](/ig/2.png)



结构 总览
============================

线总览全局，后续将以**结构体**为例，进行讲解以下概念。

1. 数据

- `数据抽象`

  就是要抽象成各种数据类型。

***

2. 数据类型

- `基本类型`(Primitive Type)

  - 在编程语言中，不可再分的**数据类型**

  - 基本类型是内置的可用类型，可以进一步细分为 多种细分类别
    - 能和算术运算符一起作用的类型，是**算数类型**；
    - 能和逻辑运算符一起作用的类型，是**标量类型**；
      - （标量类型又包括了：算数类型+指针类型。）
    - 以上细分类别都能作用于if、for等分支循环语句中的 控制表达式！；

- `复合类型`(Compound Type)

  - 由`基本类型`**组合**而成

  - 有时需要反过来，**分解**自身成各个相应`基本类型`

  - 这种即可组合，又可分解的特性，为`数据抽象`奠定了基础

  - 仅可以分为一种 **结构体类型**（以下详解！）
    - 结构体类型，不能和算术运算符一起作用的类型，所以非**算数类型**；
    - 结构体类型，不能和逻辑运算符一起作用的类型，所以非**标量类型**；
      - （标量类型又包括了：算数类型+指针类型。）
    - 结构体类型，还不能作用于if、for等分支循环语句中的 控制表达式！；

- 还有`数据类型标志`可对复合类型做适配用 Adapt。

  - 会使用到**枚举类型 enumeration**，如

    ```c
    enum some_type { TYPE1 = 1, TYPE2 };

    struct My复数结构 {
      enum some_type t;       // 给新结构体加一个 识别符，即 数据类型标志enum。（有了它，以后在结构体里编码，就能使用’标志’的变量 TYPE1、TYPE2、TYPE3...等等了！）
      double x, y;
    };
    ```

    ```c
    struct My复数结构 transformSomething(double x, double y)
    {
      struct My复数结构 z;    // 定义新结构体变量

      // 赋值
      z.t = TYPE1;    // 注意：直接使用以上 标识符 中定义的‘变量’ TYPE1、TYPE2、TYPE3...等等 来赋值！
      z.x = x;
      z.y = y;

      return z;
    }
    ```

***

3. 过程

- 过程抽象

  上一部中你看到的函数，就是过程抽象。将多条语句，封装成一个函数，成为一个整体。

***


结构体
============================

是自定义的复合类型。即 自定义 结构体 如复数类型。

- 定义 复数结构体：

  `````c
  /*
  My复数结构 就是 标识符Tag。
  同时也是一个新‘类型’，即 复数类型。
  同时也是一个 复合类型。（拥有多个成员的 新结构体！）
  */
  struct My复数结构 {
    double x, y;    // 有两个成员变量 x 和 y
  };                // 注意分号！
  `````

- 使用 复数结构体 之 复合类型 复数类型，来定义2个变量：

  `````c
  /*
  定义 My复数结构 类型的变量
  */
  struct My复数结构 {
    double x, y;
  } z1, z2;
  `````

  - 再定义2个变量：

    `````c
    /*
    再定义 My复数结构 类型的变量
    */
    struct My复数结构 z3, z4;       // 常用方式！
    `````

- 也可以不写 Tag 来定义结构体：

  `````c
  /*
  定义，且不写 Tag
  */
  struct {
    double x, y;
  } z1, z2;
  `````

  但是如此，后边后边将无法再度引用此新类型。

- 访问：

  如结构体 My复数结构，它里边的 x和y 的存储空间是紧邻着的！合在一起组成了 新结构体 的声明变量的 存储空间。

  `````c
  /*
  访问新结构体中的成员
  */
  z1.x = 1.0;
  z1.y = 2.3;
  `````

- 赋值：

  `````c
  /*
  初始化
  */
  struct My复数结构 z1 = { 1.0, 2.3 };

  // 也可以是
  struct My复数结构 z1 = { 变量, 2.3 };     // 注意：用变量来初始化自己，则要求自己z1必须是 全局变量 才能如此赋值！否则只能用常量来赋值。（C99 才有此特性）

  或少指定成员也可以，默认y是 0：

  struct My复数结构 z1 = { 1.0, };

  或用一个0也可以，默认全是 0：

  struct My复数结构 z1 = { 0 };
  `````

  - 错误的赋值方式：

    `````c
    /*
    错误的
    */
    struct My复数结构 z1;       // 系统内部的 基本类型，才可以如此
    z1 = { 1.0, 2.3 };
    `````

  - 只针对某一个成员的赋值方式：

    `````c
    /*
    只赋值y
    */
    struct My复数结构 z1 = { .y = 2.3 };        // .x 为默认的 0.0
    `````

- 注意：

  结构体，都不能使用 算术运算符，也不能使用 逻辑运算符 来参与运算，否则报错。

  同理，if 和 循环 中的`控制表达式`也不能使用 结构体。

- 结构体之间，可以相互赋值：

  `````c
  /*
  结构体之间赋值：
  */
  struct My复数结构 z1 = { 1.0, 2.3 };
  struct My复数结构 z2 = z1;
  z1 = z2;
  `````

- 结构体当函数参数，及返回值类型！：

  `````c
  /*
  当函数参数，及返回值类型
  */
  struct My复数结构 function123( struct My复数结构 z1, struct My复数结构 z2 )
  {
    ...
    return z1;
  }
  `````


数据抽象
============================

也是需要基于 新的结构体，才能做好数据抽象。

定义多个如加减乘除的若干函数时，只要确保这些函数没有直接访问 新结构体 的成员（的代码），那么这些函数也就不依赖于新结构体的成员（即不依赖结构体的变化而变化，设计上解耦！）。下边详细解释：

- 将数据`分层设计`：(从设计上，分为`3部分`来编码～)

  1. main 函数的一般**外部调用**代码；

    ```
    （即web的 controller 控制器代码，理解一下）
    ```

  2. 设计一系列**运算层**代码；

    ```
    （即web的业务逻辑层，理解吗！model模型代码）

    例如对复数 新结构体 进行的 加、减、乘、除 四则运算 的4个函数，都直接将`My复数结构`当做一个整体来访问使用，不过问其中成员x或y，如此便能解耦，分了层。
    ```

  3. 设计一系列**存储表示层**代码；

    ```
    （底层的，定义结构的代码。此处才频繁的访问新结构个体中的各成员）

    此处应该有转换函数，对结构的多种可能的 数据结构 进行转换！并供以上的 运算层 各函数调用其所需要的数据！！！例如 加减函数需要 'IP地址'，乘除函数需要 'Mac地址'。而在此处 存储表示层 设计好转换，可以随时供应各种上层所需数据～
    一句话：
    可提供各种所需的 表示。
    ```

    如果改变了结构体的实现，就要改变这一层 **存储表示层** 函的实现。但只要确保了函数接口不改变，则调用这一层函数接口的复数的**运算层**代码都不需要任何改变 即可完成变化。

- 分层设计（即 数据抽象）的作用：（重要×）

  这里复数的`存储表示层`和复数的`运算层`，都称为抽象层（Abstraction Layer）。从底层往上层来看，复数越来越抽象了。把所有这些层，组合在一起就是一个**完整的系统**。

  - （多层划分 实现的各种可能的）组合，使得系统可以任意**复杂**（令系统可千变万化！）；

  - 而`抽象`使得系统的复杂性是**可以控制的**！
    - 任何改动都只局限在某一层，而不会波及整个系统。

TODO 先做理解，日后完善此处内容。



堆栈
=======

堆栈不像数组那样可以随意访问下标中的存储，仅仅能通过 push、pop 来访问。想想汉诺塔那道题。

如果堆栈所有元素`类型相同`，其`存储`也可以使用`数组`实现！`访问`通过**函数接口**提供。（毕竟C语言的数组要求，元素类型必须相同，才可操作）

堆栈深度优先搜索
----------

TODO 需要完善代码示例。

队列
=======

先进先出。想想排队买票。

队列广度优先搜索
----------

每次都同时的从各个方向探索一步。
