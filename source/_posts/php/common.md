---
title: php常用语法
date: 2020-07-09 19:33:05
categories:
- php
---

* strip_tags

  去掉html标签

* get_current_user()

  获取当前 PHP 脚本所有者名称

* memory_get_usage()

  获取当前内存

* json_decode

  1. 没有结果

     json_last_error() 输出最后一个错误 ，4为语法错误，

     如果不是转义原因，看存储的数据是否完整，有时候字段定义过短，可用mediumtext。

* 获取路径

  ```php
  echo __FILE__ ; // 取得当前文件的绝对地址，结果：D:\www\test.php
  echo dirname(__FILE__); // 取得当前文件所在的绝对目录，结果：D:\www\
  echo dirname(dirname(__FILE__)); //取得当前文件的上一层目录名，结果：D:\
  ```

* usleep

  注：测试test_usleep.o时，不要运行其他程序。

  1.没有"usleep(ElapsedTime);"这句代码

  CPU占用率：99.9%

  PID USER PR NI VIRT RES SHR S %CPU %MEM TIME+ COMMAND

  9540 root 20 0 3368 893 760 R 99.9 0.0 0:07.52 test_usleep.o

  2.#define ElapsedTime 1       // 1微秒

  CPU占用率：99.9%

  PID USER PR NI VIRT RES SHR S %CPU %MEM TIME+ COMMAND

  9556 root 20 0 3368 872 760 R 99.9 0.0 0:05.10 test_usleep.o

  3.#define ElapsedTime 10      // 10微秒

  CPU占用率：14.0%

  PID USER PR NI VIRT RES SHR S %CPU %MEM TIME+ COMMAND

  9596 root 20 0 3368 868 760 R 14.0 0.0 0:09.99 test_usleep.o

  4.#define ElapsedTime 100     // 100微秒

  CPU占用率：11.7%

  PID USER PR NI VIRT RES SHR S %CPU %MEM TIME+ COMMAND

  9610 root 20 0 3368 868 760 S 11.7 0.0 0:05.16 test_usleep.o

  5.#define ElapsedTime 1000    // 1000微秒=1毫秒

  CPU占用率：1.7%

  PID USER PR NI VIRT RES SHR S %CPU %MEM TIME+ COMMAND

  9624 root 20 0 3368 868 760 S 1.7 0.0 0:00.79 test_usleep.o

  6.#define ElapsedTime 10000   // 10000微秒=10毫秒

  CPU占用率：0.3%

  PID USER PR NI VIRT RES SHR S %CPU %MEM TIME+ COMMAND

  9634 root 20 0 3368 868 760 S 0.3 0.0 0:00.04 test_usleep.o

  7.#define ElapsedTime 100000  // 100000微秒=100毫秒

  CPU占用率：0.0%

  PID USER PR NI VIRT RES SHR S %CPU %MEM TIME+ COMMAND

  9644 root 20 0 3368 872 760 S 0.0 0.0 0:00.00 test_usleep.o