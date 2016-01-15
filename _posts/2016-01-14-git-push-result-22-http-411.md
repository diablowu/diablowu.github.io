---
layout: post
title: "git push时错误提示：RPC failed; result=22, HTTP code = 411"
permalink: git-push-result-22-http-411
date: 2016-01-14 17:19:15
comments: false
description: "git push result 22 http 411 "
keywords: ""
categories: tech
tags:
- git
- github
---

git push时发生411 错误，通常情况下是push的内容超过了git的默认PostBuffer（默认是1M)，所以如果发现在push较大commit时又出现`result=22, HTTP code = 411`可以尝试调整
postbuffer
<!--more-->
{% highlight bash %}
git config http.postBuffer 524288000
{% endhighlight %}

  [http://stackoverflow.com/questions/12651749/git-push-fails-rpc-failed-result-22-http-code-411](http://stackoverflow.com/questions/12651749/git-push-fails-rpc-failed-result-22-http-code-411)


