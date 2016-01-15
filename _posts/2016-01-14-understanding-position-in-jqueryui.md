---
layout: post
title: "关于jqueryui 1.10中position定位的理解"
permalink: understanding-position-in-jqueryui
date: 2016-01-14 17:48:08
comments: false
description: "Understanding position in jqueryui"
keywords: ""
categories: tech
tags:
- frontend
- jquery
- jqueryui
---

jqueryui的position提供一个方便的元素对齐方案，而且不依赖jqueryui的其他组件可以单独使用，如果你有一个元素需要与另外一个元素做位置对齐或者定位可以使用position组件。


<!--more-->

    $.(".selector").position({/*定位参数*/});


## 定位参数解释

### my

被定位的元素使用自身哪个位置进行参照，default: center

>"top", "center", "bottom", "left", "right"，以及组合例如右上 top right

还可以指定附加的偏移量，使用百分比或者像素，例如：

    "right+10 top-25%"

### of

向哪个元素定位或者对齐，可以是一个selector、element、jquery element或者一个event，如果使用event对象，则pageX and pageY将会被使用，作为当前定位参照

### at

相对of元素，与其哪个位置进行定位活对齐。default: center


>"top", "center", "bottom", "left", "right"，同my的含义

### collision

当被定为元素在某个方向上溢出窗口的位置时，以何种方式重新改变它的位置。default: flip


>flip：flip元素到目标的对面一侧，然后继续进行冲突检测知道它符合窗口位置。

>fit：让元素远离窗口边界

>flipfit：First applies the flip logic, placing the element on whichever side allows more of the element to be visible. Then the fit logic is applied to ensure as much of the element is visible as possible.


通过position可以把一个元素中心和另外一个元素的右上角：
{% highlight javascript %}
$( "#positioned" ).position({
  my: "center",
  at: "right top",
  of: "#targetElement"
});
{% endhighlight %}