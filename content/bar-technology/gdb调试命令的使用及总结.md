---
date: 2015-01-31T00:30:03+08:00
description: ""
tags: ["gdb","gdb调试命令","调试命令的使用","gdb调试","gdb使用","教程","gdb总结"]
title: "gdb调试命令的使用及总结"
topics: []
draft: false
url: /post/2015-01-31
---

#gdb调试命令的使用及总结
1.基本命令

1）进入GDB　　#gdb test

　　test是要调试的程序，由gcc test.c -g -o test生成。进入后提示符变为(gdb) 。

2）查看源码　　(gdb) l

　　源码会进行行号提示。

　　如果需要查看在其他文件中定义的函数，在l后加上函数名即可定位到这个函数的定义及查看附近的其他源码。或者：使用断点或单步运行，到某个函数处使用s进入这个函数。

3）设置断点　　(gdb) b 6

　　这样会在运行到源码第6行时停止，可以查看变量的值、堆栈情况等；这个行号是gdb的行号。

 4）查看断点处情况　　(gdb) info b

　　可以键入"info b"来查看断点处情况，可以设置多个断点；

5）运行代码　　(gdb) r

6）显示变量值　　(gdb) p n

　　在程序暂停时，键入"p 变量名"(print)即可；

　　GDB在显示变量值时都会在对应值之前加上"$N"标记，它是当前变量值的引用标记，以后若想再次引用此变量，就可以直接写作"$N"，而无需写冗长的变量名；

7）观察变量　　(gdb) watch n

 在某一循环处，往往希望能够观察一个变量的变化情况，这时就可以键入命令"watch"来观察变量的变化情况，GDB在"n"设置了观察点；

8）单步运行　　(gdb) n

9）程序继续运行　　(gdb) c

　　使程序继续往下运行，直到再次遇到断点或程序结束；

10）退出GDB　　(gdb) q

 
2.断点调试

命令格式　　                      例子             　　　　　　作用

break + 设置断点的行号　　break n　　　　　　在n行处设置断点

tbreak + 行号或函数名　　tbreak n/func　　　　设置临时断点，到达后被自动删除

break + filename + 行号　　break main.c:10　　用于在指定文件对应行设置断点

break + <0x...>　　break 0x3400a　　　　　　用于在内存某一位置处暂停 

break + 行号 + if + 条件　　break 10 if i==3　　　用于设置条件断点，在循环中使用非常方便 

info breakpoints/watchpoints [n]　　info break　　n表示断点号，查看断点/观察点的情况 

clear + 要清除的断点行号　　clear 10　　　　用于清除对应行的断点，要给出断点的行号，清除时GDB会给出提示

delete + 要清除的断点编号　　delete 3　　　　用于清除断点和自动显示的表达式的命令，要给出断点的编号，清除时GDB不会给出任何提示

disable/enable + 断点编号　　disable 3　　　　让所设断点暂时失效/使能，如果要让多个编号处的断点失效/使能，可将编号之间用空格隔开

awatch/watch + 变量　　awatch/watch i　　　　设置一个观察点，当变量被读出或写入时程序被暂停 

rwatch + 变量　　　　　　rwatch i　　　　　　　　设置一个观察点，当变量被读出时，程序被暂停 

catch　　　　　　　　　　　　　　　　　　设置捕捉点来补捉程序运行时的一些事件。如：载入共享库（动态链接库）或是C++的异常 

tcatch　　　　　　　　　　　　　　　　　　只设置一次捕捉点，当程序停住以后，应点被自动删除

 
3.数据命令

display +表达式　　display a　　用于显示表达式的值，每当程序运行到断点处都会显示表达式的值 

info display　　　　　　用于显示当前所有要显示值的表达式的情况 

delete + display 编号　　delete 3　　用于删除一个要显示值的表达式，被删除的表达式将不被显示

disable/enable + display 编号　　disable/enable 3　　使一个要显示值的表达式暂时失效/使能 

undisplay + display 编号　　undisplay 3　　用于结束某个表达式值的显示

whatis + 变量　　whatis i　　显示某个表达式的数据类型

print(p) + 变量/表达式　　p n　　用于打印变量或表达式的值

set + 变量 = 变量值　　set i = 3　　改变程序中某个变量的值

　　在使用print命令时，可以对变量按指定格式进行输出，其命令格式为print /变量名 + 格式

　　其中常用的变量格式：x：十六进制；d：十进制；u：无符号数；o：八进制；c：字符格式；f：浮点数。

 
4.调试运行环境相关命令

set args　　set args arg1 arg2　　设置运行参数

show args　　show args　　参看运行参数

set width + 数目　　set width 70　　设置GDB的行宽

cd + 工作目录　　cd ../　　切换工作目录

run　　r/run　　程序开始执行

step(s)　　s　　进入式（会进入到所调用的子函数中）单步执行，进入函数的前提是，此函数被编译有debug信息

next(n)　　n　　非进入式（不会进入到所调用的子函数中）单步执行

finish　　finish　　一直运行到函数返回并打印函数返回时的堆栈地址和返回值及参数值等信息

until + 行数　　u 3　　运行到函数某一行 

continue(c)　　c　　执行到下一个断点或程序结束 

return <返回值>　　return 5　　改变程序流程，直接结束当前函数，并将指定值返回

call + 函数　　call func　　在当前位置执行所要运行的函数

 
5.堆栈相关命令

backtrace/bt　　bt　　用来打印栈帧指针，也可以在该命令后加上要打印的栈帧指针的个数，查看程序执行到此时，是经过哪些函数呼叫的程序，程序“调用堆栈”是当前函数之前的所有已调用函数的列表（包括当前函数）。每个函数及其变量都被分配了一个“帧”，最近调用的函数在 0 号帧中（“底部”帧）

frame　　frame 1　　用于打印指定栈帧

info reg　　info reg　　查看寄存器使用情况

info stack　　info stack　　查看堆栈使用情况

up/down　　up/down　　跳到上一层/下一层函数

 
6.跳转执行

jump  指定下一条语句的运行点。可以是文件的行号，可以是file:line格式，可以是+num这种偏移量格式。表式着下一条运行语句从哪里开始。相当于改变了PC寄存器内容，堆栈内容并没有改变，跨函数跳转容易发生错误。

 
7.信号命令

signal 　　signal SIGXXX 　　产生XXX信号，如SIGINT。一种速查Linux查询信号的方法：# kill -l

handle 　　在GDB中定义一个信号处理。信号可以以SIG开头或不以SIG开头，可以用定义一个要处理信号的范围（如：SIGIO-SIGKILL，表示处理从SIGIO信号到SIGKILL的信号，其中包括SIGIO，SIGIOT，SIGKILL三个信号），也可以使用关键字all来标明要处理所有的信号。一旦被调试的程序接收到信号，运行程序马上会被GDB停住，以供调试。其可以是以下几种关键字的一个或多个：
　　nostop/stop
　　　　当被调试的程序收到信号时，GDB不会停住程序的运行，但会打出消息告诉你收到这种信号/GDB会停住你的程序  
　　print/noprint
　　　　当被调试的程序收到信号时，GDB会显示出一条信息/GDB不会告诉你收到信号的信息 
　　pass 
　　noignore 
　　　　当被调试的程序收到信号时，GDB不处理信号。这表示，GDB会把这个信号交给被调试程序会处理。 
　　nopass 
　　ignore 
　　　　当被调试的程序收到信号时，GDB不会让被调试程序来处理这个信号。 
　　info signals 
　　info handle 
　　　　可以查看哪些信号被GDB处理，并且可以看到缺省的处理方式

　　single命令和shell的kill命令不同，系统的kill命令发信号给被调试程序时，是由GDB截获的，而single命令所发出一信号则是直接发给被调试程序的。

 
8.运行Shell命令

　　如(gdb)shell ls来运行ls。　　

 
9.更多程序运行选项和调试

1、程序运行参数。 
　　set args 可指定运行时参数。（如：set args 10 20 30 40 50） 
　　show args 命令可以查看设置好的运行参数。 
2、运行环境。 
　　path 可设定程序的运行路径。 
　　show paths 查看程序的运行路径。

　　set environment varname [=value] 设置环境变量。如：set env USER=hchen 

　　show environment [varname] 查看环境变量。 

3、工作目录。

　　cd　　  相当于shell的cd命令。 

　　pwd　　显示当前的所在目录。 
4、程序的输入输出。 
　　info terminal 显示你程序用到的终端的模式。 
　　使用重定向控制程序输出。如：run > outfile 
　　tty命令可以指写输入输出的终端设备。如：tty /dev/ttyb

5、调试已运行的程序

两种方法： 
　　(1)在UNIX下用ps查看正在运行的程序的PID（进程ID），然后用gdb PID格式挂接正在运行的程序。 
　　(2)先用gdb 关联上源代码，并进行gdb，在gdb中用attach命令来挂接进程的PID。并用detach来取消挂接的进程。

6、暂停 / 恢复程序运行　　当进程被gdb停住时，你可以使用info program 来查看程序的是否在运行，进程号，被暂停的原因。 在gdb中，我们可以有以下几种暂停方式：断点（BreakPoint）、观察点（WatchPoint）、捕捉点（CatchPoint）、信号（Signals）、线程停止（Thread Stops），如果要恢复程序运行，可以使用c或是continue命令。

7、线程（Thread Stops）

如果程序是多线程，可以定义断点是否在所有的线程上，或是在某个特定的线程。 
　　break thread
　　break thread if ... 
　　linespec指定了断点设置在的源程序的行号。threadno指定了线程的ID，注意，这个ID是GDB分配的，可以通过“info threads”命令来查看正在运行程序中的线程信息。如果不指定thread 则表示断点设在所有线程上面。还可以为某线程指定断点条件。如： 
　　(gdb) break frik.c:13 thread 28 if bartab > lim 
当你的程序被GDB停住时，所有的运行线程都会被停住。这方便查看运行程序的总体情况。而在你恢复程序运行时，所有的线程也会被恢复运行。

 
10.调试core文件

Core Dump：Core的意思是内存，Dump的意思是扔出来，堆出来。开发和使用Unix程序时，有时程序莫名其妙的down了，却没有任何的提示(有时候会提示core dumped)，这时候可以查看一下有没有形如core.进程号的文件生成，这个文件便是操作系统把程序down掉时的内存内容扔出来生成的, 它可以做为调试程序的参考

(1)生成Core文件
　　一般默认情况下，core file的大小被设置为了0，这样系统就不dump出core file了。修改后才能生成core文件。

　　#设置core大小为无限
　　ulimit -c unlimited
　　#设置文件大小为无限
　　ulimit unlimited

　　这些需要有root权限, 在ubuntu下每次重新打开中断都需要重新输入上面的第一条命令, 来设置core大小为无限

core文件生成路径:输入可执行文件运行命令的同一路径下。若系统生成的core文件不带其他任何扩展名称，则全部命名为core。新的core文件生成将覆盖原来的core文件。

1）/proc/sys/kernel/core_uses_pid可以控制core文件的文件名中是否添加pid作为扩展。文件内容为1，表示添加pid作为扩展名，生成的core文件格式为core.xxxx；为0则表示生成的core文件同一命名为core。
可通过以下命令修改此文件：
echo "1" > /proc/sys/kernel/core_uses_pid

2）proc/sys/kernel/core_pattern可以控制core文件保存位置和文件名格式。
可通过以下命令修改此文件：
echo "/corefile/core-%e-%p-%t" > core_pattern，可以将core文件统一生成到/corefile目录下，产生的文件名为core-命令名-pid-时间戳
以下是参数列表:
    %p - insert pid into filename 添加pid
    %u - insert current uid into filename 添加当前uid
    %g - insert current gid into filename 添加当前gid
    %s - insert signal that caused the coredump into the filename 添加导致产生core的信号
    %t - insert UNIX time that the coredump occurred into filename 添加core文件生成时的unix时间
    %h - insert hostname where the coredump happened into filename 添加主机名
    %e - insert coredumping executable name into filename 添加命令名

(2)用gdb查看core文件

　　发生core dump之后, 用gdb进行查看core文件的内容, 以定位文件中引发core dump的行.
　　gdb [exec file] [core file]
　　如:
　　gdb ./test core

　　或gdb ./a.out
 　　core-file core.xxxx
　　gdb后, 用bt命令backtrace或where查看程序运行到哪里, 来定位core dump的文件->行.

　　待调试的可执行文件，在编译的时候需要加-g，core文件才能正常显示出错信息

　　1）gdb -core=core.xxxx
　　file ./a.out
　　bt
　　2）gdb -c core.xxxx
　　file ./a.out
　　bt

(3)用gdb实时观察某进程crash信息

　　启动进程
　　gdb -p PID
　　c
　　运行进程至crash
　　gdb会显示crash信息
　　bt










#gdb调试命令说明

# 1: 对于在应用程序中加入参数进行调试的方法：
   直接用 gdb app -p1 -p2 这样进行调试是不行的。
   需要像以下这样使用：
    #gdb app
    (gdb) r -p1 -p2
    或者在运行run命令前使用set args命令：
    （gdb） set args p1 p2
    可以用show args 命令来查看

# 2. 加入断点：
   break <linenumber>
   break <funcName>
   break +offset
   break -offset
   (在当前行号的前面或后面的offset行停住。)

   break filename:linenum
   在源文件filename的linenum行处停住。

   break filename:function
   在源文件filename的function函数的入口处停住。

  break ... if
  ...可以是上述的参数，condition表示条件，在条件成立时停住。比如在循环境体中，可以设置     break if i=100，表示当i为100时停住程序。


# 3. 查看运行时的堆栈：
   使用bt命令

# 4. 打印某个变量的值：
   print val

# 5. 单步： n
   继续运行：　c
　　step
　　单步跟踪，如果有函数调用，他会进入该函数。
　　next
　　同样单步跟踪，如果有函数调用，他不会进入该函数。很像VC等工具中的step over。后面可以加count也可以不加，不加表示一条条地执行，加表示执行后面的count条指令，然后再停住。
　　set step-mode
　　set step-mode on
　　打开step-mode模式，于是，在进行单步跟踪时，程序不会因为没有debug信息而不停住。这个参数有很利于查看机器码。
　　set step-mod off
　　关闭step-mode模式。
　　finish
　　运行程序，直到当前函数完成返回。并打印函数返回时的堆栈地址和返回值及参数值等信息。
　　until 或 u
　　当你厌倦了在一个循环体内单步跟踪时，这个命令可以运行程序直到退出循环体。
 
# 6.在gdb中执行shell命令：
　在gdb环境中，你可以执行UNIX的shell的命令，使用gdb的shell命令来完成：
　eg. shell make
 
# 7. 运行环境
 可设定程序的运行路径。
  show paths 查看程序的运行路径。
  set environment varname [=value] 设置环境变量。如：set env USER=hchen
  show environment [varname] 查看环境变量。

# 8.观察点（WatchPoint）
  观察点一般来观察某个表达式（变量也是一种表达式）的值是否有变化了，如果有变化，马上停住程 序。我们有下面的几种方法来设置观察点：
  watch
   为表达式（变量）expr设置一个观察点。一量表达式值有变化时，马上停住程序。
  rwatch
   当表达式（变量）expr被读时，停住程序。
  awatch
   当表达式（变量）的值被读或被写时，停住程序。
  info watchpoints
   列出当前所设置了的所有观察点。

# 9. 维护breakpoint
   clear
    清除所有的已定义的停止点。
   clear func
    清除所有设置在函数上的停止点。
  delete [breakpoints] [range...]
  删除指定的断点，breakpoints为断点号。如果不指定断点号，则表示删除所有的断点。range 表示断点号的范围（如：3-7）。其简写命令为d。
  比删除更好的一种方法是disable停止点，disable了的停止点，GDB不会删除，当你还需要时，enable即可，就好像回收站一样。
  disable [breakpoints] [range...]
   disable所指定的停止点，breakpoints为停止点号。如果什么都不指定，表示disable所有的停止 点。简写命令是dis.
  enable [breakpoints] [range...]
   enable所指定的停止点，breakpoints为停止点号。


# 10、程序变量
查看文件中某变量的值：
file::variable
function::variable
可以通过这种形式指定你所想查看的变量，是哪个文件中的或是哪个函数中的。例如，查看文件f2.c中的全局变量x的值：
gdb) p 'f2.c'::x

查看数组的值
有时候，你需要查看一段连续的内存空间的值。比如数组的一段，或是动态分配的数据的大小。你可以使用GDB的“@”操作符，“@”的左边是第一个内存的地址的值，“@”的右边则你你想查看内存的长度。例如，你的程序中有这样的语句：
int *array = (int *) malloc (len * sizeof (int));
于是，在GDB调试过程中，你可以以如下命令显示出这个动态数组的取值：
p *array@len
如果是静态数组的话，可以直接用print数组名，就可以显示数组中所有数据的内容了。



# 10-1.输出格式
一般来说，GDB会根据变量的类型输出变量的值。但你也可以自定义GDB的输出的格式。例如，你想输出一个整数的十六进制，或是二进制来查看这个整型变量的中的位的情况。要做到这样，你可以使用GDB的数据显示格式：

x 按十六进制格式显示变量。
d 按十进制格式显示变量。
u 按十六进制格式显示无符号整型。
o 按八进制格式显示变量。
t 按二进制格式显示变量。
a 按十六进制格式显示变量。
c 按字符格式显示变量。
f 按浮点数格式显示变量。
(gdb) p i
$21 = 101
(gdb) p/a i
$22 = 0x65
(gdb) p/c i
$23 = 101 'e'
(gdb) p/f i
$24 = 1.41531145e-43
(gdb) p/x i
$25 = 0x65
(gdb) p/t i
$26 = 1100101

# 11.查看内存
使用examine命令（简写是x）来查看内存地址中的值。x命令的语法如下所示：
x/
n、f、u是可选的参数。
n 是一个正整数，表示显示内存的长度，也就是说从当前地址向后显示几个地址的内容。
f 表示显示的格式，参见上面。如果地址所指的是字符串，那么格式可以是s，如果地十是指令地址，那么格式可以是i。
u 表示从当前地址往后请求的字节数，如果不指定的话，GDB默认是4个bytes。u参数可以用下面的字符来代替，b表示单字节，h表示双字节，w表示四字节，g表示八字节。当我们指定了字节长度后，GDB会从指内存定的内存地址开始，读写指定字节，并把其当作一个值取出来。

n/f/u三个参数可以一起使用。例如：
命令：x/3uh 0x54320 表示，从内存地址0x54320读取内容，h表示以双字节为一个单位，3表示三个单位，u表示按十六进制显示。

# 12.自动显示
你可以设置一些自动显示的变量，当程序停住时，或是在你单步跟踪时，这些变量会自动显示。相关的GDB命令是display。
display
display/
display/ expr
expr是一个表达式，fmt表示显示的格式，addr表示内存地址，当你用display设定好了一个或多个表达式后，只要你的程序被停下来，GDB会自动显示你所设置的这些表达式的值。

格式i和s同样被display支持，一个非常有用的命令是：
display/i $pc

undisplay
delete display
删除自动显示，dnums意为所设置好了的自动显式的编号。

disable display
enable display
disable和enalbe不删除自动显示的设置，而只是让其失效和恢复。

info display
查看display设置的自动显示的信息。GDB会打出一张表格，向你报告当然调试中设置了多少个自动显示设置，其中包括，设置的编号，表达式，是否enable。

# 13. 设置显示选项
set print address
set print address on
打开地址输出，当程序显示函数信息时，GDB会显出函数的参数地址。系统默认为打开的，
show print address
查看当前地址显示选项是否打开。

set print array
set print array on
打开数组显示，打开后当数组显示时，每个元素占一行，如果不打开的话，每个元素则以逗号分隔。这个选项默认是关闭的。与之相关的两个命令如下，我就不再多说了。

set print array off
show print array

set print elements
这个选项主要是设置数组的，如果你的数组太大了，那么就可以指定一个来指定数据显示的最大长度，当到达这个长度时，GDB就不再往下显示了。如果设置为0，则表示不限制。

show print elements
查看print elements的选项信息。

set print null-stop
如果打开了这个选项，那么当显示字符串时，遇到结束符则停止显示。这个选项默认为off。

set print pretty on
如果打开printf pretty这个选项，那么当GDB显示结构体时会比较漂亮。

# 14.关于显示源码list

以下是list命令的說明。
參數                                     說明
list filename:number  列出某檔案的第幾行，檔案是可省略的。
list [function]                  列出某函數的程式碼
list                                       繼續印出程式碼
list -                                     印出上一次list的程式碼的前一段程式碼(類似向上翻動)
show listsize   顯示現在一次印出幾行
set listsize  設定一次印出幾行



备常用命令：
## １.常看源码：list（ｌ）　
　　list　＜linenumber＞　行号
　　list　＜＋offset＞　当前行号的正偏移
　　list　＜－offset＞　当前行号的负偏移
　　list　＜filename：linenumber＞　哪个文件的哪一行
　　list　＜function＞　函数名
　　list　＜filename：function＞　文件的哪个函数
　　list　＜＊address＞　程序运行时语句在内存中的地址

## ２．设置断点：break（ｂ）
　　break　＜function＞　指定函数断点
　　break　＜linenumber＞　指定行号断点
　　break　＜＋offset／－offset＞　当前行号的正／负偏移
　　break　＜filename：linenumber＞　哪个文件的哪一行
　　break　＜＊address＞　运行中的内存地址
　　break　不带参数，下一条指令停止处
　　break ...　if　＜condition＞　在运行中，当condition条件满足时停止。
　　　　ｅｇ．　break　if　ｉ＝100 //当i=100时，立即停止
                 break foo if i=100   //断点设置在foo中，断点条件是i-100, 一点在函数foo中，i的值等于100,被停止。
 
                

## ３．查看信息： info
　　info　break　查看断点信息
    info locals 打印出当前函数中所有局部变量及其值
　　info　stack　查看栈中信息
    info frame 更详细的栈层地址信息
　　info　args　查看参数信息
　　info　registers／info　all－registers　查看（所有）寄存器信息
　　info　sources　查看项目的源代码信息

## ４．维护breakpoint：disable/enable/clear/delete
     disable(dis) 【breakpoints】 【range...】
       如果没有参数，则停止所有的断点，
     enable 【breakpoints】【range】
     clear <function>/<filename:function>/<linenum>/<filename:linenum>
        清楚已定义的停止点
      delete [breakpoints] [ranga...]
         删除指定的断点

## ５．恢复程序运行：  
　　continue（c）
　　
## 6.until和finish
  until 跳出循环比较有用
    help finish
     Execute until selected stack frame returns.
     Upon return, the value returned is printed and put in the value history.
  finish 用来跳出函数比较有用。
    help until
     Execute until the program reaches a source line greater than the current
     or a specified location (same args as break command) within the current frame

 

##一、多线程调试  
多线程调试重要就是下面几个命令：  
info thread 查看当前进程的线程。   
thread <ID> 切换调试的线程为指定ID的线程。   
break file.c:100 thread all  在file.c文件第100行处为所有经过这里的线程设置断点。   set scheduler-locking off|on|step，这个是问得最多的。  
  * 在使用step或者continue命令调试当前被调试线程的时候，其他线程也是同时执行的，怎么只让被调试程序执行呢？通过这个命令就可以实现这个需求。   
  * off 不锁定任何线程，也就是所有线程都执行，这是默认值。   
  * on 只有当前被调试程序会执行。   
  * step 在单步的时候，除了next过一个函数的情况(熟悉情况的人可能知道，这其实是一个设置断点然后continue的行为)以外，只有当前线程会执行。 

  
##二、调试宏
这个问题超多。在GDB下，我们无法print宏定义，因为宏是预编译的。但是我们还是有办法来调试宏，这个需要GCC的配合。  
在GCC编译程序的时候，加上-ggdb3参数，这样，你就可以调试宏了。  
另外，你可以使用下述的GDB的宏调试命令 来查看相关的宏。  
info macro – 你可以查看这个宏在哪些文件里被引用了，以及宏定义是什么样的。   
macro – 你可以查看宏展开的样子。  


##三、源文件
这个问题问的也是很多的，太多的朋友都说找不到源文件。在这里我想提醒大家做下面的检查：  
编译程序员是否加上了-g参数以包含debug信息。 路径是否设置正确了。使用GDB的directory命令来设置源文件的目录。   
下面给一个调试/bin/ls的示例（ubuntu下）   

    $ apt-get sourcecoreutils  
    $ sudoapt-get installcoreutils-dbgsym  
    $ gdb /bin/ls  
    GNU gdb (GDB) 7.1-ubuntu  
    (gdb) list main  
    1192    ls.c: No such fileor directory.  
    inls.c  
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
    

##四、条件断点
条件断点是语法是：` break  [where] if [condition] `，这种断点真是非常管用。尤其是在一个循环或递归中，或是要监视某个变量。  
注意，这个设置是在GDB中的，只不过每经过那个断点时GDB会帮你检查一下条件是否满足。  


##五、命令行参数
有时候，我们需要调试的程序需要有命令行参数，很多朋友都不知道怎么设置调试的程序的命令行参数。   
其实，有两种方法：  
- gdb命令行的 –args 参数   
- gdb环境中 set args命令。   


##六、gdb的变量
有时候，在调试程序时，我们不单单只是查看运行时的变量，我们还可以直接设置程序中的变量，以模拟一些很难在测试中出现的情况，比较一些出错，或是switch的分支语句。使用set命令可以修改程序中的变量。  
另外，你知道gdb中也可以有变量吗？就像shell一样，gdb中的变量以$开头，比如你想打印一个数组中的个个元素，你可以这样：  

    (gdb) set$i = 0
      
    (gdb) p a[$i++]
      
    ...  #然后就一路回车下去了
    
当然，这里只是给一个示例，表示程序的变量和gdb的变量是可以交互的。  



##七、x命令
也许，你很喜欢用p命令。所以，当你不知道变量名的时候，你可能会手足无措，因为p命令总是需要一个变量名的。x命令是用来查看内存的，在gdb中 “help x” 你可以查看其帮助。  
- x/x 以十六进制输出  
- x/d 以十进制输出   
- x/c 以单字符输出   
- x/i  反汇编 – 通常，我们会使用 x/10i $ip-20 来查看当前的汇编（$ip是指令寄存器）  
- x/s 以字符串输出   



##八、command命令
有一些朋友问我如何自动化调试。这里向大家介绍command命令，简单的理解一下，其就是把一组gdb的命令打包，有点像字处理软件的“宏”。下面是一个示例：

    (gdb) breakfunc
    Breakpoint 1 at 0x3475678: filetest.c, line 12.
    (gdb) command1
    Type commands forwhen breakpoint 1 is hit, one per line.
    End with a line saying just "end".
    >print arg1
    >print arg2
    >print arg3
    >end
    (gdb)
    
当我们的断点到达时，自动执行command中的三个命令，把func的三个参数值打出来。    
 



#设置core环境
uname -a 查看机器参数  
ulimit -a 查看默认参数  
ulimit -c 1024  设置core文件大小为1024  
ulimit -c unlimit 设置core文件大小为无限  

  多线程如果dump，多为段错误，一般都涉及内存非法读写。可以这样处理，使用下面的命令打开系统开关，让其可以在死掉的时候生成 core文件。     
ulimit -c unlimited  



#线程调试命令
1. (gdb)info threads   
显示当前可调试的所有线程，每个线程会有一个GDB为其分配的ID，后面操作线程的时候会用到这个ID。 
前面有*的是当前调试的线程。  

2. (gdb)thread ID   
切换当前调试的线程为指定ID的线程。  

3. (gdb)thread apply ID1 ID2 command    
让一个或者多个线程执行GDB命令command。  

4. (gdb)thread apply all command   
让所有被调试线程执行GDB命令command。   

5. (gdb)set scheduler-locking off|on|step    
估计是实际使用过多线程调试的人都可以发现，在使用step或者continue命令调试当前被调试线程的时候，其他线程也是同时执行的，怎么只让被调试程序执行呢？通过这个命令就可以实现这个需求。   
- off 不锁定任何线程，也就是所有线程都执行，这是默认值。    
- on 只有当前被调试程序会执行。    
- step 在单步的时候，除了next过一个函数的情况(熟悉情况的人可能知道，这其实是一个设置断点然后continue的行为)以外，只有当前线程会执行。   

//显示线程堆栈信息    
6. (gdb) bt    
察看所有的调用栈   

7. (gdb) f 3
调用框层次  

8. (gdb) i locals  
显示所有当前调用栈的所有变量   
  
















