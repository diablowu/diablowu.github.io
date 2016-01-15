---
layout: post
title: "把博客搬家到Github Pages上了！"
permalink: move-my-blog-to-github-pages
date: 2016-01-15 15:05:42
comments: true
description: "move my blog to github pages"
keywords: ""
categories: others
tags:
- welcome
---

把博客搬家到Github Pages上了，也算是乔迁之喜了。
<!--more-->

#### 为什么要搬到这里？
首先gh-pages的稳定性毋庸置疑，如果哪天被墙了这个另说。再加上jekyll强大的功能，
一个稳定高效的静态博客非他莫属啊。下面是几个优点：

  1. git管理，完整保持文章的每次变更
  2. markdown编写，书写流畅程度上比哪些所见即所得的功能强多了，是的就只是书写
  3. 静态的！静态的！静态的！重要的事情说三遍！看看WordPress我都想笑
  4. 模板化，这个是必须的，jekyll提供了简单基于ruby的DSL的模板语言去定制自己的主题
  5. jekyll天生支持语法高亮：pygments插件
  6. 继续发现待补充......

#### 为什么没有评论？
原来的主题是带`disqus`模块的，不过让我给删了，准备后面把`多说`加进来

#### Todo list

  1. 用NodeWebkit写个图片上传qiniu的小工具
  2. rakefile加入备份任务
  3. 文章页好看点
  4. 寻找一个好看的favicon
