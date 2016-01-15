---
layout: post
title: "Ubuntu Server 12.04 初始化月份字符串出错 的解决方法"
permalink: ubuntu-format-string-error
date: 2016-01-14 17:41:22
comments: false
description: "ubuntu format string error"
keywords: ""
categories: tech
tags:
- ubuntu
- linux
---


{% highlight bash %}
diablo@host:sudo vi /etc/default/locale
{% endhighlight %}

修改文件内容如下：
<!--more-->



    LANG="zh_CN.UTF-8"
    LANGUAGE="zh_CN:zh"
    LC_NUMERIC="zh_CN.UTF-8"
    LC_TIME="zh_CN.UTF-8"
    LC_MONETARY="zh_CN.UTF-8"
    LC_PAPER="zh_CN.UTF-8"
    LC_NAME="zh_CN.UTF-8"
    LC_ADDRESS="zh_CN.UTF-8"
    LC_TELEPHONE="zh_CN.UTF-8"
    LC_MEASUREMENT="zh_CN.UTF-8"
    LC_IDENTIFICATION="zh_CN.UTF-8"
