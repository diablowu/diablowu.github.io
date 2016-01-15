---
layout: post
title: "Nginx 反向代理到后端appserver导致cookie丢失的解决办法"
permalink: nginx-proxy-pass-loss-cookies
date: 2016-01-15 14:22:44
comments: false
description: "nginx proxy pass loss cookies"
keywords: ""
categories: tech
tags:
- nginx
- cookie
---

### 问题描述
nginx的反向代理proxy_pass可以把后端的多个appserver通过反向代理的方式在前端为用户展示为一个统一的入口，将后端多个appserver屏蔽，比如我们可以把
`http://ip/app1/`上得所有请求都通过proxy_pass转发到后端的`http://127.0.0.1:8080/`app上，但是如果这里使用了servlet的会话管理，就会出现一些问题。

<!--more-->

我们知道默认的session管理是通过保存一个`JSESSIONID`的cookie到浏览器，但是实际上设置`setcookie`的是后端的appserver，即`127.0.0.1:8080`这个tomcat等，这样如果我们的app的context是 /app ,8080服务设置的cookie的path是 /app ,以至于最终通过`http://ip/app1/`得到的response中得到的setcookie头的path是 `/app` ,但是实际上通过`http://ip/app1/`返回的请求`setcookie`的path应该是`/app1`，这样以来虽然response里面接收到了`JSESSIONID`的`setcookie`，但是实际上由于path不一致会导致浏览器不会为http://ip/app1/ 生成cookie数据，这样就导致后面如果再做请求，也就不会携带上次返回的cookie中的`JSESSIONID`。

### 解决方案
其实这也是浏览器同源策略导致的，只要我们把path设置为和url一直的值即可

#### 土鳖点的
直接解决`JSESSIONID`的传值问题，直接在url后面拼接`JSESSIONID`，但是这样会导致会话token暴露，存在会话劫持的风险

#### 洋气点的
通过nginx的[`proxy_cookie_path`](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cookie_path)指令，替换cookie的path

