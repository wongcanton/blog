---
id: 703
title: 编程珠玑番外篇-4. Linux 下的 Facade 程序
date: 2008-11-17T23:21:43+00:00
author: Eric
comments: true
layout: post
guid: http://blog.youxu.info/?p=703
permalink: /2008/11/17/facade-commands-in-linux/
dsq_thread_id:
  - 336567088
categories:
  - CompSci
  - Cool Stuff
  - pearl
tags:
  - ffmpeg
  - gcc
  - Linux
---
Linux 下的命令行工具大致有两个流派, 一是以小而精见长的, 只能提供一个简单的小功能. 比如 yes 这个命令, 除了输出一大串永不停止的 y 之外毫无用处. 这个工具看上去土, 很没用处的样子. 碰到要你一路回车法的时候, 这个工具就大大的有用. 所以我每次帮人使用一路回车法装 windows 的时候, 就怀恋 Linux 下的这个 yes. 过一个管道, 就省去了在电脑面前按下几百次 y 的繁复工作. 

还有一种工具, 是我今天要说的重点. 这种工具一般是一个简单的命令行调用, 却有着几十种甚至上百种不同的参数的组合, 用这些参数能搭配出谁也没用过的功能. 以 gcc 为例, 居然有两百多个不同的命令行参数, 范围涉及到程序编译, 连接设置, 库设置, 优化, 报错信息, 调试信息等等, 任何一个正常的人想要穷尽学完这些参数都是不可能的. 同样的库还有 convert (图像转换的), ffmpeg (视频处理的), curl (内容抓取的). 看上去这些参数指示的功能乱七八糟的堆砌在一起的样子, 仔细一想这些功能的确是相互关联的, 所以被放到了一个工具之下. 这些工具和上面的工具的哲学是反其道而行之的: 集一大类功能于一个工具, 任何类似的操作都能通过这个一个命令+不同的参数来完成, 而非&#8221;do one thing, do it well&#8221;. 这些工具和传统意义上的 UNIX 工具哲学是不大像的. 为了区分他们, 我把它们叫做 Facade 工具, 因为这些工具的设计哲学很类似于 Design Pattern 里面的 [Facade Pattern](http://en.wikipedia.org/wiki/Facade_pattern) (Facade 模式的核型是用一个统一的接口管理对一个系统的访问. 比如 gcc 就是对整个编译系统的接口, ffmpeg 就是对整个视频处理系统的接口, display 就是对整个 X 显示系统的接口等等.)

之所以区分这两者, 是我体会到: 在具体的学习过程中, 对付两者的学习方法是截然不一样的. 学习小工具, 基本上就是学一个简单的名字到功能的定义, 加一些简单的参数. 除了名字比较别扭外, 使用很方便, 学习曲线不陡峭. 学习的要点不在于这些小工具本身, 而在于利用管道和其他工具通信(小工具从来就不是单独使用的, 比如 yes, 比如 tr, 我几乎没见过不用管道的情况下用他们的); 和上面相反的是, 我几乎没见着 Facade 工具用在管道里面的.

原因是 Facade 工具基本上是一个自成体系的完整的操作方式, 就像一个新的领域的一种新的&#8221;语言&#8221;一样. 因此, 不掌握一点基本的编译知识, 就不可能把 gcc 玩转, 因为那些参数的含义的理解, 都是需要相应知识的. 我也常常看到不少 做 Web 程序的哥们对 curl 的每个边边角角都很熟悉, 但是对 gcc 不太熟, 这也是很正常的, 因为 Facade 程序本来就是属于面向一个特定领域的工具. 

我在学习这两种截然不同的工具的时候也曾感到过困惑: 怎么有的程序这么多参数, 全学会怎么可能. 在浪费了不少时间乱看这些 Facade 程序的 man 文件之后, 我认识到: 除非我写操作系统, 要让我的程序编译的时候有几百个参数, 否则, 简简单单的用 gcc 常用参数就能解决99%的问题了. 我觉得, Facade 程序的要点正是在于, 用一些简单的参数组合(更多情况下其实不要参数) 就可以完成 90% 的常用例子. 至于剩下的 10%, 遇到了再去查文档就行了. 同时, 对于不在自己&#8221;常用工具集&#8221;中的一些 Facade 工具, 认真学习他们的用法是一件非常耗时且几乎没有任何收获的事情, 而且学到的也不会被实际用到. 所以, 千万不要被&#8221;获取新知识的成就感&#8221; 给蒙蔽了, 去钻研那些琐碎的边边角角. 

而对于小工具, 却要反过来. 我觉得在学习小工具 (尤其是 [coreutils](http://en.wikipedia.org/wiki/GNU_Core_Utilities) 里面的所有命令) 的时候, 最好要做个有心人, 把大部分参数弄清楚记住 (本来参数也不多). Linux 下的小工具基本上是千锤百炼经过无数进化的, 应该说每个选项都是很常用的. 搞明白这些选项, 可以极大化发挥这些小工具的优势, 还能提高自己的生产率. 举个例子: 比如说 ssh 这个程序, 90% 的哥们就是用他来登录服务器, 然后运行服务器上的某个程序. 其实 ssh 的文档写得很清楚, 你可以把 ssh 后面接一个命令文件. 比如说 

> ssh name@server.com ls

就可以直接显示服务器上的目录了. 还可以拓展一下, 

> ssh name@server.com < script.py

就可以直接把本机上的 script.py 放在服务器上跑, 无需把文件先拷贝过去. (走题一下: 跨平台的脚本语言的好处就在这里. Apache 的 Hadoop 是 MapReduce 的一个开源实现, 他的任务控制器就是采用我说的这种方式来调用各个机器上的Mapper 或者 Reducer 工作的). 因此, 掌握 ssh 的加命令的用法, 在我看来, 是值得的.

很多小工具都有这样不太鲜为人知的用法, 熟稔这些用法, 我觉得是值得的, 况且这也不需要花多少时间, 只要打印一份文档每天睡前看半页就行了.  我以前还有整理了不少这类平时大多数人注意不到的小命令的一些&#8221;黑魔法&#8221;. 我觉得这些黑魔法一点都不是什么奇技淫巧, 而是实实在在能提高效率的魔法, 是居家旅行必备的工具套装. 

PS: 最近有几个朋友看了我的博客, 发信让我推荐学习 Linux 的书. 我推荐 &#8220;鸟哥的Linux私房菜&#8221; 这本书. 我学 Linux 的过程中没看过这本书, 所以折腾的比较曲折. 直到我大四我才看到这本书, 这本书是一本非常深入浅出的好书. 

PS2: GNU 的工具链有把小工具 Facade 化的倾向. 连 ls 这么简单的命令都有几十个参数. 在这种情况下, 还是挑选一些认为会常用的参数学习一下就行了, 没有必要去追求高大全. 一般说来, 这种两个字母的小工具, 如果后面加的参数超过6个字母, 就完全不对味了. 工具这东西, 强极则无用至极. 

-EOF-