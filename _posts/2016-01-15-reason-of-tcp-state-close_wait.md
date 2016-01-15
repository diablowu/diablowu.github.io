---
layout: post
title: "聊聊TCP状态中的CLOSE_WAIT"
permalink: reason-of-tcp-state-close_wait
date: 2016-01-15 11:11:11
comments: true
description: "reason of tcp state close_wait"
keywords: ""
categories: tech
tags:
- tcp
---




>CLOSE_WAIT，TCP的癌症，TCP的朋友。
<!--more-->

CLOSE_WAIT状态的生成原因
首先我们知道，如果我们的服务器程序APACHE处于CLOSE_WAIT状态的话，说明套接字是被动关闭的！

因为如果是CLIENT端主动断掉当前连接的话，那么双方关闭这个TCP连接共需要四个packet：

    Client --->  FIN  --->  Server

    Client <---  ACK  <---  Server

这时候Client端处于FIN_WAIT_2状态；而Server 程序处于CLOSE_WAIT状态。

    Client <---  FIN  <---  Server

这时Server 发送FIN给Client，Server 就置为LAST_ACK状态。

    Client --->  ACK  --->  Server

Client回应了ACK，那么Server 的套接字才会真正置为CLOSED状态。

Server 程序处于CLOSE_WAIT状态，而不是LAST_ACK状态，说明还没有发FIN给Client，那么可能是在关闭连接之前还有许多数据要发送或者其他事要做，导致没有发这个FIN packet。

通常来说，一个CLOSE_WAIT会维持至少2个小时的时间。如果有个流氓特地写了个程序，给你造成一堆的CLOSE_WAIT，消耗

你的资源，那么通常是等不到释放那一刻，系统就已经解决崩溃了。

只能通过修改一下TCP/IP的参数，来缩短这个时间：修改tcp_keepalive_*系列参数有助于解决这个问题。
