---
title: 番外：PUBG与Intel的10nm
date: 2019-11-07 14:14:48
categories:
  - 番外
tags:
  - PUBG
  - Intel
---

今天 PUBG 的运营在微博发了一条很有趣的消息，是这样的：

{% asset_img weibo.png 这是微博的H5版，微博按时间轴显示 %}

<!-- more -->

正好我昨晚在玩，也有出现闪退现象，不过我的笔电 CPU 是 i5-4210H ，而 Icelake 是10代，抱着看戏的心态点开了看看。

具体的临时解决方式是这样的：

{% asset_img env.png 估计跟着教程学 Java 的人会很得心应手 %}

通常来说设置的系统变量并不长这样，很多人熟悉的多数都是目录，而值的部分很明显是内存偏移量，于是我就把变量名拿去搜了。首先是搜到这段

```assembly
unsigned long *OPENSSL_ia32cap_loc(void);
#define OPENSSL_ia32cap ((OPENSSL_ia32cap_loc())[0])
env OPENSSL_ia32cap=~0x1000000
```

然后还有[这个]( https://s0www0openssl0org.icopy.site/docs/man1.1.0/man3/OPENSSL_ia32cap.html ) OpenSSL 的文档。

基本可以确认 `OPENSSL_ia32cap`这个值是个处理器功能向量。

然后，我试着研究了一下把这个值设成图里的值是干什么用的。

0x200000200000000 转换到二进制就是

```
‭1011 0101 1110 0110 0010 1100 1110 0000 0100 0010 0000 0000‬
```

按照我在学校学的鬼知识……最右边那位应该是第0位。

>  bit #28 denoting Hyperthreading, which is used to distinguish cores with shared cache; 

28位是0……

可以理解成这是关闭了超线程吗……？（大概率不是吧w）

不过我对碰到内存的东西都很苦手，姑且先把这个值当成我记得的那种功能向量来理解，这样一想，或许 Intel 10nm 的架构与 14nm++ 有一些大的区别，当中可能涉及到游戏引擎优化。

如果我的猜想正确，部分使用到这个值……也就是其它使用到 OpenSSL 来进行安全通讯的游戏，或是别的什么东西一律会……出事。

那么现在被多人使用的游戏引擎么……当属 Unity 和 UE4 ，先看 Unity ，在 Manual 里的 [NetworkConnection]( https://docs.unity3d.com/Manual/class-NetworkConnection.html ) 部分能找到这句话：

> The Transport Layer can use two protocols: UDP for generic communications, and WebSockets for WebGL. 

同时自己看了看 `Networking.NetworkTransport` ，里面没有提到 OpenSSL 。

接着看 Unreal4 （别忘了 PUBG 就是这引擎做的），文档里找不到有关的描述，那就换种方式搜，我试着在谷歌里搜类似「ue4 using openssl」的字句，找到了 UE4 官方论坛里的一个[帖](https://forums.unrealengine.com/unreal-engine/feedback-for-epic/1503637-openssl-error-when-starting-ue4editor )，在 #2 回复又看到了熟悉的关键词。

>  Until the OpenSSL fix makes it into the launcher, you can create an environment variable called "OPENSSL_ia32cap" with the value ":~0x20000000d". (Both without the quote marks) 

这个帖里的问题是打不开 Epic Games Launcher，并且「 I then tried starting UE4Editor.exe but it crashed without error or showing up. 」

那么会是 OpenSSL 的问题吗？但 OpenSSL 最近的更新是在 9 月。而根据 Intel 官网的数据里看， Icelake ，也就是 10 代 Core ，目前只有 G 后缀，通通都是移动平台，推出日期都是 Q3'19……

不，我觉得就是 Intel 的 10nm 架构问题。

或者说我最初就觉得是 Intel 在 10nm 的架构上改动了什么东西，导致和原本的 14nm++ 相差太大而造成的。

