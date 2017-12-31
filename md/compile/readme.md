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

make
----------

- MakeFile 中如何定义 make clean：

  ```bash
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
