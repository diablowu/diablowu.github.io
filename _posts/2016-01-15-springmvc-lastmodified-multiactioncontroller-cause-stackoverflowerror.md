---
layout: post
title: "springmvc中使用MultiActionController定义lastModified方法可能造成stackoverflowerror"
permalink: springmvc-lastmodified-multiactioncontroller-cause-stackoverflowerror
date: 2016-01-15 11:35:06
comments: false
description: "springmvc lastModified MultiActionController cause stackoverflowerror"
keywords: ""
categories: tech
tags:
- java
- spring
- springmvc
---

## 背景介绍
springmvc有一种使用方式，是controller继承`MultiActionControlle`r，然后通过定义methodNameResolver的paraName来到指定的方法来执行业务action。例如，在请求index.do?method=get的时候就是通过spring的`DispatcherServlet`被派发到`FrameworkServlet`，然后通过反射调用get方法。


### 问题重现
使用上面的配置，如果请求index.do?method=get，那么后台会发生大量的递归调用，直到递归太深超过Stack Space限制，发生`StackOverflowError`.

<!--more-->

![stackoverflow](http://7af4fj.com1.z0.glb.clouddn.com/img/image2013-5-14-11-35-46.png)

涉及springmvc版本，目前只测试了两个版本

**3.0.5.RELEASE**和**3.2.2.RELEASE**
### 分析
`MultiActionController`实现了`LastModified`接口，这个接口由`MultiActionController`实现，内部通过反射构造一个方法名，格式是

`{methodName}LastModified`，这个方法应该由自己的controller来实现，目的是计算LastModified，资源的最后修改时间，以便让浏览器缓存此资源，如果没有找到这样的方法，就返回-1。但是如果正巧你的方法名叫get那么根据上面的格式，最终`MultiActionController`会找`getLastModified`这样方法，认为是你自定义的LastModified，但是`getLastModified`方法是由`MultiActionController`实现，目的是去寻找自定义的`LastModified`方法，这样就造成了一个无限递归调用。

### 结论 **起名的学问**
>不要使用get作为MultiActionController方式的方法。

>set/get/execute/perform从来都不是一个很好的方法名，尤其是在javaee中。

>合理设置jvm的xss参数，防止深度递归造成DoS。