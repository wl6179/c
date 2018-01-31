[返回目录](/README.md)

第八部 摩呼罗迦，逍遥

![第八部 摩呼罗迦，逍遥](/ig/8.jpg)


PHP扩展
===========================

准备
----------

- 安装PHP扩展开发环境

  ```bash
  apt-get	install	php-dev
  ```

- 己手动下载	PHP	源码

  访问官网下载自己需要的版本。


调试方法
----------

- 使用 gdb 工具，初步调试PHP异常错误

  - **测试`程序跑崩`了之后， 会`生成`一个 `core 文件`：**

    用 gdb 打开 `core 文件`

      ```bash
      gdb php coreFile
      ```

    用` bt 命令`查看是从哪行崩溃的就可以了。

    注意：

      如果你的程序跑崩了却没有产生 core 文件，你可以试试执行 ulimit -c unlimited 这条命令，然后再执行你的程序，你一般就会找到那个 core 文件了，如果还找不到，那就到 /cores，/tmp 或 /var/tmp 下看看有没有，在这些目录下的 core 文件通常会带一个数字的后缀，你只要知道就是那个东西就行了。


  - **或者`直接用 gdb 跑崩`一个 PHP 程序：**

    先启动 gdb

      ```bash
      gdb php
      ```

    然后运行 PHP 程序

      ```bash
      run test.php
      ```

    如果执行崩了，你会直接看到崩溃信息！

    TODO 更多

  - **使用 valgrind 工具，进一步调试错误信息：**

    如果 gdb 很难定位出错位置、测试程序没有跑崩却出现莫名其妙的情况、或内存泄漏时，就该 valgrind 出场了。

    例如

    ```bash
    USE_ZEND_ALLOC=0 valgrind php -n -c '/var/www/home/project_c/test.php'
    ```

    如果你的测试程序没有跑通过，在测试目录下，就会生成一个跟测试文件同名但扩展名是 .sh 的文件。打开这个文件，把里面的命令行复制出来贴到 USE_ZEND_ALLOC=0 valgrind 的后面就可以了。

    作用：

      ```
      如果你的程序中有内存泄漏，或者有重复的内存释放，或者释放了不该释放的值，那么你就可以看到这些错误的详细信息了。
      ```


安装扩展
----------

- 安装方法

  ```bash
  cd /path/to/extend/
  phpize            # 主要步骤，确保你安装好了上一步的 php-dev
  ./configure
  make
  make install
  ```


可使用工具
----------

  - 使用 `PHP扩展的脚手架生成工具ext_skel` 生成PHP扩展框架（源码）

    在源码的 ext 目录下，找到 ext_skel	的程序。他用来生成一个扩展的`源代码框架`。

    ```bash
    cd /path/to/ext/
    ./ext_skel --extname=NBA


    Creating directory NBA
    Creating basic files: config.m4 config.w32 .gitignore NBA.c php_NBA.h CREDITS EXPERIMENTAL tests/001.phpt NBA.php [done].

    To use your new extension, you will have to execute the following steps:

    1.  $ cd ..
    2.  $ vi ext/NBA/config.m4
    3.  $ ./buildconf
    4.  $ ./configure --[with|enable]-NBA
    5.  $ make
    6.  $ ./sapi/cli/php -f ext/NBA/NBA.php
    7.  $ vi ext/NBA/NBA.c
    8.  $ make

    Repeat steps 3-6 until you are satisfied with ext/NBA/config.m4 and
    step 6 confirms that your module is compiled into PHP. Then, start writing
    code and repeat the last two steps as often as necessary.
    ```

    此时立刻生成一个新目录 NBA/。

    如果想深入了解 NBA/ 目录下的若干C文件，可以去官网查看两块内容：[构建系统](http://php.net/manual/zh/internals2.buildsys.php)、[扩展的结构](http://php.net/manual/zh/internals2.structure.php)。

  - 使用 现成的扩展 拷贝修改修改，作为一个新的PHP扩展

    在源码的 ext 目录下，随意复制某个扩展。

  - 使用 Netbeans 导入PHP扩展项目（C/C++）

    例如

      1.使用 git clone 下载了PHP源代码；

      2.进入目录；

      3.打开 NB，新建项目；

        选择为 基于现有源代码 C/C++ 项目

        目录中如果没有执行 phpize，则执行一下 phpize 即可让 NB 顺利往下创建项目（不会提示报错找不到 makefile 或配置脚本！）

      ok

- 扩展的构成

  例
