[返回目录](/README.md)

指针例子
===========================


字符串`数组`的表示
----------

`msg[] `表示不定长的`（数组元素）个数`，即若干个`字符串`组成；

`char *` 表示（每个）字符串的`指针地址`；

所以 `char *msg[]` 表示字符`*串`的，数组集合。

```c
// 仔细理解指针的含义：
...
char *msg[] = {"Sunday", "Monday", "Tuesday", "Wednesday",
			"Thursday", "Friday", "Saturday"};
...
```

我再举一个例子，就会更清晰：

```c
// 再仔细理解，此时`指针`的含义：
char *msg = "Sunday0014575678...";
// 还记得在上一章曾经提过，char *msg 指针，与不定长数组几乎是等价的。即 与 char msg[] 等价。
```