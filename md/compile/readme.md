[返回目录](/README.md)

编译
===========================

MakeFile 自动更新依赖 完成编译
----------

- 怎么写 MakeFile：

  MakeFile 由多个`规则`组成。

  ```
  每条规则的格式：（command 命令处是 TAB开头，不是空格）

  target 目标: prerequisites 条件 ...
  	command1 命令
  	command2 命令
  	...
  ```

- 例如：

  ```bash
  #缺省目标（判断依据：main文件是否已生成）
  main: main.o stack.o maze.o     # 必须先更新了main.o、stack.o和maze.o这三个条件，然后才能更新main
  	gcc main.o stack.o maze.o -o main

  #条件1（需要更新判断依据：main.o文件是否已生成）
  main.o: main.c main.h stack.h maze.h
  	gcc -c main.c

  #条件2（需要更新判断依据：stack.o文件是否已生成）
  stack.o: stack.c stack.h main.h
  	gcc -c stack.c

  #条件3（需要更新判断依据：maze.o文件是否已生成）
  maze.o: maze.c maze.h main.h
  	gcc -c maze.c
  ```

  假如改写了里边的某一个.c文件，此时 make 会判断，最近更改的`文件的修改时间`是谁，然后在 MakeFile 中查找到对应文件名。如果有，则更新对应的`目标`！

    1.比如我修改了文件 stack.h，那么先判断找出哪个`文件’最新’`；

    2.然后根据 MakeFile 的规则我们`查出对应的目标`为：stack.o、main.o、main；

    3.最后执行相应命令；

- MakeFile 更新的机制：

  更新`目标`时，必须首先更新它的所有`条件`；

  所有条件中，只要有一个`条件`被更新了，`目标`也必须随之被更新；

  make 时，会创建一个`Shell进程`去执行每一个需要执行的命令。

- MakeFile 还可`以条件`为中心来写：（让多目标，来依赖一`条件`！重心改变）

  ```vim
  main: main.o stack.o maze.o
  	gcc main.o stack.o maze.o -o main

  main.o stack.o maze.o: main.h         #多个目标，同时的依赖同一个`条件`！
  main.o maze.o: maze.h
  main.o stack.o: stack.h
  ```

  总之，MakeFile 的唯一目的，就是描述清楚`依赖关系`，怎么写都无所谓！ 其实，`多目标`的规则，make会自动拆成几条`单目标`的规则来处理。

- 变量和赋值：

  一般用等于号 `=` 时，变量在`前、后`定义都可以，因为 = 不会立刻`展开`；

  但是我们有时候希望 make 在遇到变量定义时`立即展开`，可以用 `:=` 运算符；

make
----------

- MakeFile 中如何定义 make clean：

  ```bash
  ...
  # 注意：命令处开头用的是 TAB
  clean:
  	@echo "cleanning project"        #执行的命令前面，加了@字符，则不显示命令本身而只显示它的结果；如果这条命令出错，make不会继续执行后续命令。
  	-rm main *.o                     #执行的命令前面，加了-字符，则显示命令本身，且显示它执行结果；即使这条命令出错，make也会继续执行后续命令。
  	@echo "clean completed"
  ```

  执行 make：

  ```bash
  make clean
  ```

- make 的几个约定俗成的`目标`：

    `all`，执行主要的编译工作，通常用作缺省目标。

    `install`，执行编译后的安装工作，把可执行文件、配置文件、文档等分别拷到不同的安装目录。

    `clean`，删除编译生成的二进制文件。

    `distclean`，终极清空。不仅删除编译生成的二进制文件，也删除其它生成的文件，只留下源文件。

- make 的隐含规则数据库：

  ```bash
  make -p
  ```

  截取一段有用的：

  ```vim
  ...
  # default
  OUTPUT_OPTION = -o $@

  # default
  CC = cc

  # default
  COMPILE.c = $(CC) $(CFLAGS) $(CPPFLAGS) $(TARGET_ARCH) -c

  %.o: %.c
  #  commands to execute (built-in):
          $(COMPILE.c) $(OUTPUT_OPTION) $<
  ...
  ```
- makefile 自动生成依赖关系：（-M）

  ```bash
  gcc -M main.c
  ```

  或

  ```bash
  gcc -MM *.c
  ```

make 的测试方法：
----------

就是`不真的执行`，而是大概`输出一下`将要执行的`命令`行出来！。

```bash
gcc -n *.c
```
