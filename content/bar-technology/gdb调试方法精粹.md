---
date: 2015-01-31T00:30:03+08:00
description: ""
tags: ["Hugo","静态网站生成器","博客","教程","网站"]
title: "Hugo静态网站生成器中文教程"
topics: []
draft: false
url: /post/2015-01-31
---


#一、多线程调试
###1. 多线程调试,最重要的几个命令:
info threads                        查看当前进程的线程。
                                          GDB会为每个线程分配一个ID, 后面操作线程的时候会用到这个ID.
                                          前面有*的是当前调试的线程.
thread                      切换调试的线程为指定ID的线程。
break file.c:100 thread all    在file.c文件第100行处为所有经过这里的线程设置断点。
set scheduler-locking off|on|step    
      在使用step或者continue命令调试当前被调试线程的时候，其他线程也是同时执行的,
      怎么只让被调试程序执行呢？
      通过这个命令就可以实现这个需求。
         off      不锁定任何线程，也就是所有线程都执行，这是默认值。
         on       只有当前被调试程序会执行。
         step     在单步的时候，除了next过一个函数的情况
                  (熟悉情况的人可能知道，这其实是一个设置断点然后continue的行为)以外，
                  只有当前线程会执行。
thread apply ID1 ID2 command        让一个或者多个线程执行GDB命令command
thread apply all command            让所有被调试线程执行GDB命令command。

###2. 使用示例:
线程产生通知：在产生新的线程时, gdb会给出提示信息
>(gdb) r
Starting program: /root/thread 
[New Thread 1073951360 (LWP 12900)] 
[New Thread 1082342592 (LWP 12907)]---以下三个为新产生的线程
[New Thread 1090731072 (LWP 12908)]
[New Thread 1099119552 (LWP 12909)]

查看线程：使用info threads可以查看运行的线程。
>(gdb) info threads
  4 Thread 1099119552 (LWP 12940)   0xffffe002 in ?? ()
  3 Thread 1090731072 (LWP 12939)   0xffffe002 in ?? ()
  2 Thread 1082342592 (LWP 12938)   0xffffe002 in ?? ()
* 1 Thread 1073951360 (LWP 12931)   main (argc=1, argv=0xbfffda04) at thread.c:21
(gdb)

注意，行首为gdb分配的线程ID号，对线程进行切换时，使用该ID号码。
另外，行首的星号标识了当前活动的线程
切换线程：
使用 thread THREADNUMBER 进行切换，THREADNUMBER 为上文提到的线程ID号。
下例显示将活动线程从 1 切换至 4。
>(gdb) info threads
   4 Thread 1099119552 (LWP 12940)   0xffffe002 in ?? ()
   3 Thread 1090731072 (LWP 12939)   0xffffe002 in ?? ()
   2 Thread 1082342592 (LWP 12938)   0xffffe002 in ?? ()
* 1 Thread 1073951360 (LWP 12931)   main (argc=1, argv=0xbfffda04) at thread.c:21
(gdb) thread 4
[Switching to thread 4 (Thread 1099119552 (LWP 12940))]#0   0xffffe002 in ?? ()
(gdb) info threads
* 4 Thread 1099119552 (LWP 12940)   0xffffe002 in ?? ()
   3 Thread 1090731072 (LWP 12939)   0xffffe002 in ?? ()
   2 Thread 1082342592 (LWP 12938)   0xffffe002 in ?? ()
   1 Thread 1073951360 (LWP 12931)   main (argc=1, argv=0xbfffda04) at thread.c:21
(gdb)

以上即为使用gdb提供的对多线程进行调试的一些基本命令。
另外，gdb也提供对线程的断点设置以及对指定或所有线程发布命令的命令

#二、调试宏
在GDB下, 我们无法print宏定义，因为宏是预编译的。
但是我们还是有办法来调试宏，这个需要GCC的配合。
在GCC编译程序的时候，加上
  -ggdb3   参数，这样，你就可以调试宏了。

另外，你可以使用下述的GDB的宏调试命令 来查看相关的宏。
info macro   查看这个宏在哪些文件里被引用了，以及宏定义是什么样的。
macro         查看宏展开的样子。

#三、源文件
GDB时,提示找不到源文件。
需要做下面的检查:
编译程序员是否加上了 -g参数 以包含debug信息。
路径是否设置正确了。
使用GDB的directory命令来设置源文件的目录。

下面给一个调试/bin/ls的示例(ubuntu下)
>$ apt-get source coreutils
$ sudo apt-get install coreutils-dbgsym
$ gdb /bin/ls
GNU gdb (GDB) 7.1-ubuntu
(gdb) list main
1192    ls.c: No such file or directory.
in ls.c
(gdb) directory ~/src/coreutils-7.4/src/
Source directories searched: /home/hchen/src/coreutils-7.4:$cdir:$cwd
(gdb) list main
1192        }
1193    }
1194
1195    int
1196    main (int argc, char **argv)
1197    {
1198      int i;
1199      struct pending *thispend;
1200      int n_files;
1201

#四、条件断点
条件断点是语法是：
  break  [where] if [condition]
这种断点真是非常管用。
尤其是在一个循环或递归中，或是要监视某个变量。
注意，这个设置是在GDB中的，只不过每经过那个断点时GDB会帮你检查一下条件是否满足。

#五、命令行参数
有时候，我们需要调试的程序需要有命令行参数, 有三种方法：
gdb命令行的 -args 参数
gdb环境中   set args命令。
gdb环境中   run 参数

#六、gdb的变量
有时候，在调试程序时，我们不单单只是查看运行时的变量，
我们还可以直接设置程序中的变量，以模拟一些很难在测试中出现的情况，比较一些出错，
或是switch的分支语句。使用set命令可以修改程序中的变量。
另外，你知道gdb中也可以有变量吗？
就像shell一样，gdb中的变量以$开头，比如你想打印一个数组中的个个元素，你可以这样：
(gdb) set $i = 0
(gdb) p a[$i++]
...  #然后就一路回车下去了
当然，这里只是给一个示例，表示程序的变量和gdb的变量是可以交互的。

#七、x命令
也许，你很喜欢用p命令。
所以，当你不知道变量名的时候，你可能会手足无措，因为p命令总是需要一个变量名的。
x命令是用来查看内存的，在gdb中 “help x” 你可以查看其帮助。
x/x 以十六进制输出
x/d 以十进制输出
x/c 以单字符输出
x/i  反汇编 – 通常，我们会使用 x/10i $ip-20 来查看当前的汇编（$ip是指令寄存器）
x/s 以字符串输出

#八、command命令
如何自动化调试。
这里向大家介绍command命令，简单的理解一下，其就是把一组gdb的命令打包，有点像字处理软件的“宏”。
下面是一个示例：
>(gdb) break func
>Breakpoint 1 at 0x3475678: file test.c, line 12.
>(gdb) command 1
>Type commands for when breakpoint 1 is hit, one per line.
>End with a line saying just "end".
>print arg1
>print arg2
>print arg3
>end
>(gdb)

当我们的断点到达时，自动执行command中的三个命令，把func的三个参数值打出来。



