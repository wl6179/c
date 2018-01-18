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

  - 测试程序跑崩了之后， 会生成一个 core 文件：

    用 gdb 打开 core 文件

      ```bash
      gdb php core文件
      ```

    用 bt 命令查看是从哪行崩溃的就可以。

    注意：

      如果你的程序跑崩了却没有产生 core 文件，你可以试试执行 ulimit -c unlimited 这条命令，然后再执行你的程序，你一般就会找到那个 core 文件了，如果还找不到，那就到 /cores，/tmp 或 /var/tmp 下看看有没有，在这些目录下的 core 文件通常会带一个数字的后缀，你只要知道就是那个东西就行了。

  - 或者，直接用 gdb 跑崩一个 PHP 程序：

    先启动 gdb

      ```bash
      gdb	php
      ```

    然后运行 PHP 程序

      ```bash
      run test.php
      ```

    如果执行崩了，你会直接看到崩溃信息！

    TODO 更多

  - 使用 valgrind 工具，进一步调试错误信息

    如果 gdb 很难定位出错位置、测试程序没有跑崩却出现莫名其妙的情况、或内存泄漏时，就该 valgrind 出场了。

    例如

    ```bash
    USE_ZEND_ALLOC=0 valgrind /path/to/php -n -c '/work/hprose-pecl/tmp-php.ini'
    ```

    如果你的测试程序没有跑通过，在测试目录下，就会生成一个跟测试文件同名但扩展名是 .sh 的文件。打开这个文件，把里面的命令行复制出来贴到 USE_ZEND_ALLOC=0 valgrind 的后面就可以了。

    作用：

      ```
      如果你的程序中有内存泄漏，或者有重复的内存释放，或者释放了不该释放的值，那么你就可以看到这些错误的详细信息了。
      ```