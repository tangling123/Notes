# CMakeLists





# GDB

[一个小时玩转GDB](http://c.biancheng.net/gdb/)

![GDB调试器](http://c.biancheng.net/uploads/allimg/200729/2-200H9100455Z6.gif)GNU symbolic debugger，简称「**GDB 调试器**」，是 Linux 平台下最常用的一款程序调试器。GDB 编译器通常以 gdb 命令的形式在终端（Shell)中使用，它有很多选项，这是我们要重点学习的。

发展至今，GDB 调试器已经对 C、C++、Go、Objective-C、OpenCL、Ada 等多种编程语言提供了支持。实际场景中，GDB 更常用来调试 C 和 C++ 程序，虽然 Linux 平台下有很多能编写 C、C++ 代码的集成开发工具（IDE），但它们调试代码的能力往往都源自 GDB 调试器。

调试是开发流程中一个非常重要的环境，每个程序员都应具备调试代码的能力，尤其对于从事 Linux C/C++ 开发的读者，必须具备熟练使用 GDB 调试器的能力。这套 GDB 入门教程通俗易懂，深入浅出，能让你快速学会使用 GDB 编译器。

> 阅读本教程需要读者具备 C/C++ 基础，如果你还不了解 C/C++，请转到《[C语言入门教程](http://c.biancheng.net/c/)》《[C++入门教程](http://c.biancheng.net/cplus/)》。



从现在开始，我将系统教大家学习使用 GDB，本节先解决第一个问题，即 GDB 是什么。

要知道，哪怕是开发经验再丰富的程序员，编写的程序也避免不了出错。程序中的错误主要分为 2 类，分别为语法错误和逻辑错误：

- 程序中的语法错误几乎都可以由编译器诊断出来，很容易就能发现并解决；
- 逻辑错误指的是代码思路或者设计上的缺陷，程序出现逻辑错误的症状是：代码能够编译通过，没有语法错误，但是运行结果不对。对于这类错误，只能靠我们自己去发现和纠正。

也就是说，程序中出现的语法错误可以借助编译器解决；但逻辑错误则只能靠自己解决。实际场景中解决逻辑错误最高效的方法，就是借助调试工具对程序进行调试。

所谓调试（Debug），就是让代码一步一步慢慢执行，跟踪程序的运行过程。比如，可以让程序停在某个地方，查看当前所有变量的值，或者内存中的数据；也可以让程序一次只执行一条或者几条语句，看看程序到底执行了哪些代码。

也就是说，通过调试程序，我们可以监控程序执行的每一个细节，包括变量的值、函数的调用过程、内存中数据、线程的调度等，从而发现隐藏的错误或者低效的代码。

> 对于初学者来说，学习调试可以增加编程的功力，能让我们更加了解自己的程序，比如变量是什么时候赋值的、内存是什么时候分配的，从而弥补学习的纰漏。调试是每个程序员必须掌握的基本技能，没有选择的余地！

就好像编译程序需要借助专业的编译器，调试程序也需要借助专业的辅助工具，即调试器（Debugger）。表 1 罗列了当下最流行的几款调试器：



| 调试器名称      | 特 点                                                        |
| --------------- | ------------------------------------------------------------ |
| Remote Debugger | Remote Debugger 是 VC/VS 自带的调试器，与整个IDE无缝衔接，使用非常方便。 |
| WinDbg          | 大名鼎鼎的 Windows 下的调试器，它的功能甚至超越了 Remote Debugger，它还有一个命令行版本（cdb.exe），但是这个命令行版本的调试器指令比较复杂，不建议初学者使用。 |
| LLDB            | XCode 自带的调试器，Mac OS X 下开发必备调试器。              |
| GDB             | Linux 下使用最多的一款调试器，也有 Windows 的移植版。        |

本教程讲解的就是 GDB 调试器。

## 1. GDB是什么

GDB 全称“GNU symbolic debugger”，从名称上不难看出，它诞生于 GNU 计划（同时诞生的还有 GCC、Emacs 等），是 Linux 下常用的程序调试器。发展至今，GDB 已经迭代了诸多个版本，当下的 GDB 支持调试多种编程语言编写的程序，包括 C、C++、Go、Objective-C、OpenCL、Ada 等。实际场景中，GDB 更常用来调试 C 和 C++ 程序。

> Windows 操作系统中，人们更习惯使用一些已经集成好的开发环境（IDE），如 VS、VC、Dev-C++ 等，它们的内部已经嵌套了相应的调试器。

![GDB的吉祥物：弓箭鱼](http://c.biancheng.net/uploads/allimg/200212/1-2002122135363V.gif)
图 1 GDB 的吉祥物：弓箭鱼

总的来说，借助 GDB 调试器可以实现以下几个功能：

1. 程序启动时，可以按照我们自定义的要求运行程序，例如设置参数和环境变量；
2. 可使被调试程序在指定代码处暂停运行，并查看当前程序的运行状态（例如当前变量的值，函数的执行结果等），即支持断点调试；
3. 程序执行过程中，可以改变某个变量的值，还可以改变代码的执行顺序，从而尝试修改程序中出现的逻辑错误。

> 后续章节会做以上功能做详细的讲解，这里简单了解一下即可，不必深究。

正如从事 Windows C/C++ 开发的一定要熟悉 Visual Studio、从事 Java 开发的要熟悉 Eclipse 或 IntelliJ IDEA、从事 Android 开发的要熟悉 Android Studio、从事 iOS 开发的要熟悉 XCode 一样，从事 Linux C/C++ 开发要熟悉 GDB。

另外，虽然 Linux 系统下读者编写 C/C++ 代码的 IDE 可以自由选择，但调试生成的 C/C++ 程序一定是直接或者间接使用 GDB。可以毫不夸张地说，我所做那些 C/C++ 项目的开发和调试包括故障排查都是利用 GDB 完成的，调试是开发流程中一个非常重要的环节，因此对于从事 Linux C/C++ 的开发人员熟练使用 GDB 调试是一项基本要求。

“工欲善其事、必先利其器”，作为一名合格的软件开发者，至少得熟悉一种软件开发工具和调试器， 而对于 Linux C/C++ 后台开发，舍 GDB 其谁。



## 2. GDB下载和安装教程

基于 Linux 系统的免费、开源，衍生出了多个不同的 Linux 版本，比如 Redhat、CentOS、Ubuntu、Debian 等。这些 Linux 发行版中，有些默认安装有 GDB 调试器，但有些默认不安装。

判断当前 Linux 发行版是否安装有 GDB 的方法也很简单，就是在命令行窗口中执行 gdb -v 命令。以本机安装的 CentOS 系统为例：

```sh
[root@bogon ~]# gdb -v
bash: gdb: command not found
```

如上所示，执行结果为“command not found”，表明当前系统中未安装 GDB 调试器。反之，若执行结果为：

```sh
[root@bogon ~]# gdb -v
GNU gdb (GDB) Red Hat Enterprise Linux (7.2-92.el6)
Copyright (C) 2010 Free Software Foundation, Inc.
....... <-省略部分信息
```

则表明当前系统安装了 GDB 调试器。

对于尚未安装 GDB 的 Linux 发行版，安装方法通常有以下 2 种：

1. 直接调用该操作系统内拥有的 GDB 安装包，使用包管理器进行安装。此安装方式的好处是速度快，但通常情况下安装的并非 GDB 的最新版本；
2. 前往 GDB 官网下载源码包，在本机编译安装。此安装方式的好处是可以任意选择 GDB 的版本，但由于安装过程需要编译源码，因此安装速度较慢。

> 注意，不同的 Linux 发行版，管理包的工具也不同。根据维护团体（商业公司维护和社区组织维护）的不同，可以将众多 Linux 发行版分为 2 个系列，分别为 RedHat 系列和 Debian 系列。其中 RedHat 系列代表 Linux 发行版有 RedHat、CentOS、Fedora 等，使用 yum 作为包管理器；Debian 系列有 Debian、Ubuntu 等，使用 apt 作为包管理器。

### 快速安装GDB

对于 RedHat 系列的 Linux 发行版，通过在命令行窗口中执行`sudo yum -y install gdb`指令，即可实现 GDB 调试器的安装。这里以 CentOS 为例，执行该指令的过程为：

```sh
[root@bogon ~]# gdb -v
bash: gdb: command not found                        <--当前系统中没有GDB 
[root@bogon ~]# sudo yum -y install gdb               <--安装 GDB
Loaded plugins: fastestmirror, refresh-packagekit, security
Loading mirror speeds from cached hostfile
......  <-省略部分过程
Installed:
 gdb.x86_64 0:7.2-92.el6                           

Complete!
[root@bogon ~]# gdb -v
GNU gdb (GDB) Red Hat Enterprise Linux (7.2-92.el6)        <--安装成功
```

可以看到，GDB 安装成功。

对于 Debian 系列的 Linux 发行版，通过执行`sudo apt -y install gdb`指令，即可实现 GDB 的安装。感兴趣的读者可自行验证，这里不再过多赘述。

> 注意，使用 yum 或者 apt 更多情况需要借助网络下载所需使用的安装包，这也就意味着，如果读者所用系统没有网络环境，很有可能安装失败。



