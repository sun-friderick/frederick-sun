---
date: 2015-11-29T00:30:03+08:00
description: ""
tags: ["gdb","调试","gdb调试","教程","调试教程"]
title: "gdb调试"
topics: []
draft: false
url: /post/2015-11-29-7
---


# gdb 跟踪调试命令整理 

# gdb常用命令：

|                命令        | 描述           | Cool  |  
| --------------------------- |:-------------:|  
| backtrace（或bt）    |    查看各级函数调用及参数，运行时的堆栈   |   
| finish                              |    连续运行到当前函数返回为止，然后停下来等待命令    |     
| frame（或f）帧编号  |   选择栈帧       |  
| info（或i） locals       |   查看当前栈帧局部变量的值   |  
| list（或l）                     |    列出源代码，接着上次的位置往下列，每次列10行       |  
| list 行号                          |   列出从第几行开始的源代码        |  
| list 函数名                     |    列出某个函数的源代码   |  
| next（或n）                 |    单步调试，执行下一行语句        |  
| print（或p）                |    打印表达式的值，通过表达式可以修改变量的值或者调用函数     |  
| quit（或q）                 |     退出gdb调试环境  |  
| set var                           |     修改变量的值       |  
| start                               |     开始执行程序，停在main函数第一行语句前面等待命令       |  
| step（或s）                 |     执行下一行语句，如果有函数调用则进入到函数中  |  


#GDB 调试

要进行调试程序首先要生成一个含有调试信息的执行程序命令如下:

    gcc(g++) -g -o 文件名 源文件.c(源文件.cpp)
    gcc(g++) -ggdb3 -o 文件名 源文件.c(源文件.cpp)

此时便会生成一个含有调试信息的可执行文件,然后便可以用 gdb 去调试这个程序了,进入调试程序命令,但是如果用 gdb 去调试一个未包含调试信息的可执行文件则会发生错误

    gdb 执行文件名(含调试信息)

# 运行 GDB
 
## 运行 gdb

    gdb <program> -- program也就是你的执行文件,一般在当然目录下.
    gdb <program> core -- 用 gdb 同时调试一个运行程序和 core 文件,core 是程序非法执行后 core dump 后产生的文件.
    gdb <program> <PID> -- 调试正在运行的程序. program 为需要调试的程序文件, PID 为当前正在运行的程序.或是先用 gdb <program> 关联上源代码进入 gdb,后用 attach 命令来挂接进程的 PID.并用 detach 来取消挂接的进程

## gdb 启动常用的参数

    从指定文件中读取符号表
        -symbols <file>
        -s <file>
    从指定文件中读取符号表信息,并把他用在可执行文件中
        -se file
    调试时 core dump 的 core 文件
        -core <file>
        -c <file>
    加入一个源文件的搜索路径.默认搜索路径是环境变量中 PATH 所定义的路径
        -directory <directory>
        -d <directory>
    设置启动时候参数
        --args arglist

## gdb 帮助文档

    help -- 查看 gdb 的命令种类
    help <CmdType> -- 查看 CmdType 种类的 gdb 命令
    apropos <keyWord> -- 查看关键字 keyWord 的相关命令
    info <keyWord> -- 查看关键字 keyWord 调试信息
    show <keyWord> -- 查看关键字 keyWord gdb 本身设置信息

## gdb 中运行 unix 的 shell 程序

    shell <command string> -- 调用 unix 的 shell 来执行 <command string>,环境变量 shell 中定义的 unix 的 shell 将会被用来执行 <command string>,如果 shell 没有定义,那就使用 unix 的标准 shell：/bin/sh.(在 windows 中使用 command.com 或 cmd.exe)
    make <make-args> -- 等价于 “shell make <make-args>”

## 历史记录

    当你用 gdb 的 print 查看程序运行时的数据时,你每一个 print 都会被 gdb 记录下来.gdb 会以 $1, $2, $3 ...这样的方式为你每一个 print 命令编上号.于是,你可以使用这个编号访问以前的表达式,如 $1.这个功能所带来的好处是,如果你先前输入了一个比较长的表达式,如果你还想查看这个表达式的值,你可以使用历史记录来访问,省去了重复输入.

## 启动程序

    run <arg ...> -- 启动程序,<arg ...> 为程序运行时候需要输入的参数.也可用 set args 命令去设置运行参数.简写为 r

# GDB 环境设置

## gdb 设置

    设置显示选项
        地址
            set print address <on/off> -- 打开地址输出,当程序显示函数信息时,gdb会显出函数的参数地址.系统默认为打开.
            show print address -- 查看 print address 选项信息
        数组元素单独行显示
            set print array <on/off> -- 打开时数组显示时,每个元素占一行,如果不打开的话,每个元素则以逗号分隔.这个选项默认是关闭的.
            show print array -- 查看 print array 选项信息
        显示数组元素显示
            set print elements <number-of-elements> -- 设置数组的显示的最大长度,设置为 0,则表示不限制.
            show print elements -- 查看 print elements 选项信息.
        设置字符串显示
            set print null-stop <on/off> -- 如果打开那么当显示字符串时,遇到结束符则停止显示.这个选项默认为 off.
            show print null-stop -- 查看 print null-stop 选项信息
        设置结构体变量显示
            set print pretty <on/off> -- 结构体优雅显示
            show print pretty -- 查看 gdb 是如何显示结构体的.
        设置字符显示
            set print sevenbit-strings <on/off> -- 符显示,是否按“/nnn”的格式显示,如果打开,则字符串或字符数据按/nnn显示,如“/065”.
            show print sevenbit-strings -- 查看字符显示开关是否打开.
        设置联合体显示
            set print union <on/off> -- 设置显示结构体时,是否显式其内的联合体数据.
            show print union -- 查看联合体数据的显示方式
        设置对象显示
            set print object <on/off> -- 在 C++ 中,如果一个对象指针指向其派生类,如果打开这个选项,gdb 会自动按照虚方法调用的规则显示输出,如果关闭这个选项的话,gdb 就不管虚函数表了.这个选项默认是 off.
            show print object -- 查看对象选项的设置.
        设置静态成员显示
            set print static-members <on/off> -- 这个选项表示,当显示一个 C++ 对象中的内容是,是否显示其中的静态数据成员.默认是 on.
            show print static-members -- 查看静态数据成员选项设置.
        设置虚函数表显示
            set print vtbl <on/off> -- 当此选项打开时,gdb 将用比较规整的格式来显示虚函数表时.其默认是关闭的.
            show print vtbl -- 查看虚函数显示格式的选项.
    设置运行程序的相关环境及其参数
        指定源文件的路径
            directory <dirname ... > -- 加一个源文件路径到当前路径的前面.如果你要指定多个路径,UNIX 下你可以使用“:”,Windows 下你可以使用“;”.缩写 dir
            directory -- 清除所有的自定义的源文件搜索路径信息.
            show directories -- 显示定义了的源文件搜索路径.
        运行参数
            set args -- 可指定运行时参数
            show args -- 命令可以查看设置好的运行参数
        运行环境
            path <dir> -- 可设定程序的运行路径
            show paths -- 查看程序的运行路径
            set environment varname=value -- 设置环境变量
            show environment [varname] -- 查看环境变量
        工作目录
            cd <dir> -- 相当于shell的cd命令
            pwd -- 显示当前的所在目录
        程序的输入输出
            info terminal -- 显示你程序用到的终端的模式
            tty -- 命令可以指写输入输出的终端设备
            重定向控制程序输出
        堆栈帧设置
            set backtrace <limit> -- 设置堆栈帧的最大显示数量,默认是没有限制
        调试模式
            set step-mode [on | off] -- step-mode 模式,于是,在进行单步跟踪时,程序不会因为没有debug信息而不停住.这个参数有很利于查看机器码. 

# GDB 源码查看

## 显示源代码

    list <linenum> -- 显示程序第 linenum 行的周围的源程序.
    list <function> -- 显示函数名为 function 的函数的源程序.
    list <filename:linenum> -- 哪个文件的哪一行.
    list <filename:function> -- 哪个文件中的哪个函数.
    list <offset> -- 当前行号的正/负 offset 偏移量那那行.
    list -- 显示当前行后面的源程序.
    list - -- 显示当前行前面的源程序.
    list <first>, <last> -- 显示从 first 行到 last 行之间的源代码.若无 first 则显示从当前行到 last 之间的源代码.

## 设置和获得显示源码的行数

    set listsize <count> -- 设置一次显示源代码的行数.
    show listsize -- 查看当前listsize的设置.

## 搜索源代码

    forward-search <regexp> -- 向后面搜索.正则表达式为 regexp 的关键字
    search <regexp> -- 向后面搜索.正则表达式为 regexp 的关键字
    reverse-search <regexp> -- 向前面搜索.正则表达式为 regexp 的关键字

## 源代码的内存

    info line <linenum> -- 查看行号为 linenum 源代码在内存中的地址.
    info line <function> -- 查看函数在源代码在内存中的地址.
    info line <filename:linenum> -- 查看 filename 文件的第 linenum 行源代码在内存中的地址.
    info line <filename:function> -- 查看 filename 文件的 function 函数在源代码在内存中的地址.

## 查看汇编代码

    disassemble -- 查看源程序的当前执行时的机器码,这个命令会把目前内存中的指令 dump 出来.

# GDB 停止点设置及维护

## 断点(BreakPoint)

    设置断点:(threadno 指定了线程的 ID,注意,这个 ID 是 gdb 分配的,可以通过 "info threads" 命令来查看正在运行程序中的线程信息)
        break thread <threadno> -- break命令没有参数时,表示在下一条指令处停住.
        break +offset thread <threadno> -- 在当前行号的后面的 offset 行停住.(offiset 为自然数)
        break -offset thread <threadno> -- 在当前行号的前面的 offset 行停住.(offiset 为自然数)
        break <linenum> thread <threadno> -- 在指定行号停住.
        break filename:linenum thread <threadno> -- 在源文件filename的linenum行处停住.
        break <function> thread <threadno> -- 在进入指定函数时停住.
        break filename:function thread <threadno> --在源文件filename的function函数的入口处停住.
        break *address -- 在程序运行的内存地址处停住.
        break ... thread <threadno> if <condition> -- ...可以是上述的参数,condition表示条件,在条件成立时停住.比如在循环境体中,可以设置break if i=100,表示当i为100时停住程序.
        tbreak -- 设置只停止一次的断点.用法和 break 类似
    查看断点
        info breakpoints [n]
        info break [n]

## 观察点(WatchPoint) -- 观察点一般来观察某个表达式(变量也是一种表达式)的值是否有变化了,如果有变化,马上停住程序.

    设在观察点
        watch <expr> -- 为表达式(变量)expr设置一个观察点.一量表达式值有变化时,马上停住程序.
        rwatch <expr> -- 当表达式(变量)expr被读时,停住程序.
        awatch <expr> -- 当表达式(变量)的值被读或被写时,停住程序.
    查看观察点
        info watchpoints -- 列出当前所设置了的所有观察点.

## 捕捉点(CatchPoint) -- 设置捕捉点来补捉程序运行时的一些事件.如:载入共享库(动态链接库)或是 C++ 的异常《/p>

    设置捕捉点
        catch <event> -- 当event发生时,停住程序.event可以是下面的内容：
            throw 一个 C++ 抛出的异常.(throw 为关键字)
            catch 一个 C++ 捕捉到的异常.(catch 为关键字)
            exec 调用系统调用 exec 时.(exec 为关键字,目前此功能只在 HP-UX 下有用)
            fork 调用系统调用 fork 时.(fork 为关键字,目前此功能只在 HP-UX 下有用)
            vfork 调用系统调用 vfork 时.(vfork 为关键字,目前此功能只在 HP-UX 下有用)
            load 或 load <libname> 载入共享库(动态链接库)时.(load 为关键字,目前此功能只在 HP-UX 下有用)
            unload 或 unload <libname> 卸载共享库(动态链接库)时.(unload 为关键字,目前此功能只在 HP-UX 下有用)
        tcatch <event> -- 只设置一次捕捉点,当程序停住以后,应点被自动删除.

## 维护停止点

    清除停止点
        clear -- 所有的已定义的停止点.
        clear <function> -- 清除所有设置在函数上的停止点.
        clear <filename:function> -- 清除所有设置在函数上的停止点.
        clear <linenum> -- 清除所有设置在指定行上的停止点.
        clear <filename:linenum> -- 清除所有设置在指定行上的停止点.
    删除停止点
        delete [range] -- 删除停止点.其简写命令为 d.
    禁用停止点
        disable [range] -- 禁用停止点
    启用停止点
        enable [range] -- 启用停止点.
        enable once [rang] -- 启用停止点一次,当程序停止后,该停止点马上被 gdb 自动 disable.
        enable count [rang] -- 启用停止点 count 次,当程序停止后,该停止点马上被 gdb 自动 disable.
        enable delete [rang] -- 启用停止点一次,当程序停止后,该停止点马上被 gdb 自动删除.
    停止条件维护 -- 以用 condition 命令来修改断点的条件.(只有break和watch命令支持if,catch目前暂不支持if)
        condition <bnum> <expression> -- 修改断点号为bnum的停止条件为expression.
        condition <bnum> -- 清除断点号为bnum的停止条件.
    忽略停止点 N 次
        ignore <bnum> <count> -- 表示忽略断点号为 bnum 的停止条件 count 次.
    为停止点设定运行命令
        格式:
        commands [bnum]
               ... command-list ...
               // 为断点号 bnumi写一个命令列表.当程序被该断点停住时,gdb 依次运行命令列表中的命令.
        end
        例如：
        break foo if x>0
        　　commands
               printf "x is %d/n",x
               continue
        end

# 信号(Signals)

## 添加信号处理

    handle <signal> <keywords...>
    在 gdb 中定义一个信号处理.信号 <signal> 可以以 SIG 开头或不以 SIG 开头,可以用定义一个要处理信号的范围(如：SIGIO-SIGKILL,表示处理从 SIGIO 信号到 SIGKILL 的信号,其中包括 SIGIO, SIGIOT, SIGKILL 三个信号),也可以使用关键字 all 来标明要处理所有的信号.一旦被调试的程序接收到信号,运行程序马上会被 gdb 停住,以供调试.其 <keywords> 可以是以下几种关键字的一个或多个.若没有 keywords 则查看奇信号的处理状态
        nostop -- 当被调试的程序收到信号时,gdb 不会停住程序的运行,但会打出消息告诉你收到这种信号.
        stop -- 当被调试的程序收到信号时,gdb 会停住你的程序.
        print -- 当被调试的程序收到信号时,gdb 会显示出一条信息.
        noprint -- 当被调试的程序收到信号时,gdb 不会告诉你收到信号的信息.
        pass -- 当被调试的程序收到信号时,gdb 不处理信号.这表示,gdb 会把这个信号交给被调试程序会处理.
        noignore -- 当被调试的程序收到信号时,gdb 不处理信号.这表示,gdb 会把这个信号交给被调试程序会处理.
        nopass -- 当被调试的程序收到信号时,gdb 不处理信号.这表示,gdb 会把这个信号交给被调试程序会处理.
        1gnore -- 当被调试的程序收到信号时,gdb 不会让被调试程序来处理这个信号.

## 查看处理信号

    info signals -- 查看有哪些信号在被 gdb 检测中.
    info handle -- 查看有哪些信号在被 gdb 检测中.

# GDB 程序调试

## 恢复执行

    continue [ignore-count] -- ignore-count 表示忽略其后的断点次数.恢复程序运行,直到程序结束,或是下一个断点到来.缩写 c
    fg [ignore-count] -- ignore-count 表示忽略其后的断点次数.恢复程序运行,直到程序结束,或是下一个断点到来.缩写 c

## 单步调试

    step <count> -- 单步跟踪,如果有函数调用,它会进入该函数.count 表示执行后面 count 条语句,不加则默认为 1.
    next <count> -- 同样单步跟踪,如果有函数调用,他不会进入该函数.count 表示执行后面 count 条语句,不加则默认为 1.

## 跟踪机器指令
       与之一样有相同功能的命令是 “display/i $pc” ,当运行完这个命令后,单步跟踪会在打出程序代码的同时打出机器指令(也就是汇编代码)

    stepi 或 si -- 单步跟踪一条机器指令,简写 si
    nexti 或 ni -- 单步跟踪一条机器指令,简写 ni

## 函数调试

    finish -- 运行程序,直到当前函数完成返回.并打印函数返回时的堆栈地址和返回值及参数值等信息.
    return <expression> -- 使函数以 expression 表达式返回出去,忽略还没有执行的语句.若无返回 void 出去
    call <expr> -- 表达式中可以一是函数,以此达到强制调用函数的目的.并显示函数的返回值,如果函数返回值是void,那么就不显示.
    print 与 printf 也可以做到类似的功能和 call 的不同是,如果函数返回 void,call 则不显示,print 则显示函数返回值,并把该值存入历史数据中.

## 循环体调试

    until -- 可以运行程序直到退出循环体.简写 u

## 修改变量值

    print varname=var -- 修改被调试程序运行时的变量值
    set var varname=value -- 修改被调试程序运行时的变变量
    whatis varname -- 查看变量的类型

## 跳转执行

    jump <linespec> -- 指定下一条语句的运行点.<linespce> 可以是文件的行号,可以是 file:line 格式,可以是 +num 这种偏移量格式.表式着下一条运行语句从哪里开始.
    jump <address> -- 跳转到指定的程序内存地址运行.<address> 是代码行的内存地址.
    注意
        jump 指令不会改变当前的程序栈中的内容,所以,当你从一个函数跳到另一个函数时,当函数运行完返回时进行弹栈操作时必然会发生错误.
        jump 命令只是改变了指令寄存器中的值.于是可以使用 “set $pc” 来更改跳转执行的地址.如： set $pc = 0x485

## 产生信号量

    signal <signal> -- 产生一个 signal 信号.UNIX 的系统信号量通常从 1 到 15.所以 <signal> 取值也在这个范围.

# GDB 运行是数据

## 查看运行时数据

    print /<f> <expr> -- 查看当前程序的运行数据.简写 p.
        <expr> 是表达式
            @ -- 是一个和数组有关的操作符,在后面会有更详细的说明.在 @ 左边是数组的地址,右边是数组的长度,eg: array@len
            :: -- 指定一个在文件或是一个函数中的变量.
            {<type>} <addr> --表示一个指向内存地址<addr>的类型为type的一个对象.
        <f>是输出的格式
            d -- 按十进制格式显示变量.
            u -- 按十六进制格式显示无符号整型.
            o -- 按八进制格式显示变量.
            t -- 按二进制格式显示变量.
            a -- 按十六进制格式显示变量.
            c -- 按字符格式显示变量.
            f -- 按浮点数格式显示变量.
        注意:
            1.若出现变量重名,局部变量会隐藏全局变量.若想查看全局变量的值时,可以使用“::”操作符.
            2.可以通过这种形式指定你所想查看的变量 eg:
                *'filename'::variable
                *function::variable
    printf "fmt",arg,... -- 打印格式化字符灿 fmt.

## 查看内存

    examine/<n/f/u> <addr> -- 来查看内存地址中的值.简写 x
        <n/f/u>
            n 是一个正整数,表示显示内存的长度,也就是说从当前地址向后显示几个地址的内容.
            f 表示显示的格式
                s -- 按字符串格式显示内存地址内容.
                i -- 查看内存地址的机器指令内容
                x -- 按十六进制格式显示地址内容.
                d -- 按十进制格式显示地址内容.
                u -- 按十六进制格式显示无符号整型.
                o -- 按八进制格式显示地址内容.
                t -- 按二进制格式显示地址内容.
                a -- 按十六进制格式显示地址内容.
                c -- 按字符格式显示地址内容.
                f -- 按浮点数格式显示地址内容.
            u 表示从当前地址往后请求的字节数,如果不指定的话,gdb默认是 4 个 bytes.
                b -- 表示单字节
                h -- 表示双字节
                w -- 表示四字节
                g -- 表示八字节
        <addr> 表示一个内存地址.

## 自动显示

    设置自动显示
        display/<fmt> <expr> -- 自动显示 expr 表达式
        display/<fmt> <addr> -- 自动显示 addr 地址
            fmt 表示显示的格式
            i -- 输出格式为机器指令码,也就是汇编.
            s -- 输出格式为字符串
    删除自动显示
        undisplay <range> -- 删除自动显示
        delete display <range> -- 删除自动显示
    启用和禁自动显示
        sable display <range> -- 禁用自动显示
        enable display <range> -- 启动自动显示

## 查看栈信息

    backtrace <n> -- 擦看函数栈信息,简写 bt
        n 若是正数,只打印栈顶上 n 层的栈信息,
        若是负数,只打印栈顶下 n 层的栈信息,
        若无则打印当前的函数调用栈的所有信息
    frame -- 会打印出这些信息:栈的层编号,当前的函数名,函数参数值,函数所在文件及行号,函数执行到的语句.
    info frame -- 这个命令会打印出更为详细的当前栈层的信息,只不过,大多数都是运行时的内内地址.
    info args -- 打印出当前函数的参数名及其值.
    info locals -- 打印出当前函数中所有局部变量及其值.
    info catch -- 打印出当前的函数中的异常处理信息.

## 切换函数栈位置

    frame <n> -- 切换到第 n 层函数栈位置,简写 f.
    p <n> -- 表示上面移动 n 层,可以不打 n,表示向上移动一层.
    down <n> -- 表示向栈的下面移动 n 层,可以不打 n,表示向下移动一层.
    select-frame <n> -- 类似 frame 命令.不打印出栈层信息.
    up-silently <n> -- 类似 up 命令.不打印出栈层信息.
    down-silently <n> -- 类似 down 命令.不打印出栈层信息.

## 查看寄存器

    info registers -- 查看寄存器的情况.(除了浮点寄存器)
    info all-registers -- 查看所有寄存器的情况.(包括浮点寄存器)
    info registers <regname> -- 查看所指定的寄存器的情况.

## 线程查看与切换线程

    info threads -- 查看当前线程
    thread <threadno> -- 切换到 threadno 的线程,简写 t

# 自定义命令

## 定义一个命令

    格式
    define comdName
          ...
    end
    条件语句
    if cond_expr
          ...
    else
          ...
    end
    循环语句
    while cond_expr
          ...
    end

## 定义一个命令的文档信息（在 help cmdName 的时候显示）

    document cmdName
          ...
    end

## 查看自定命令

    help user-define -- 查看所有用户自定义的命令
    show user cmdName -- 查看用户定义的 cmdName 的命令.
    help cmdName -- 查看用户自定义的 cmdName 的帮助文档
    show max-user-call-depth -- 查看用户自定义命令的递归最大深度,缺省是 1024
    set-user-call-depth <limit>-- 设置用户自定义命令的递归最大深度.





