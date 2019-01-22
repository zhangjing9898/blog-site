---
layout: post
title: "flex布局"
date: 2018-05-08
categories:
  - 踩坑记
tags:
  - CSS
  - 布局
description: 之前学 react native 的时候接触过 flex，这次又系统的练习了一次，更加熟悉
---

# flex布局

之前学 react native 的时候接触过 flex，这次又系统的练习了一次，更加熟悉。
这里推荐阮一峰[flex 实战](http://www.ruanyifeng.com/blog/2015/07/flex-examples.html)

这篇文章[flex 的语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)

之前面试滴滴的时候问到了 flex：1 的意思
当时竟然没回答上来。
flex 是 flex grow flex shrink flex basis 的简写 可以写 flex：1 1 auto 后两项为可选项
flex1 就是按个数等比分配
flex2 就是 2 倍

#### flex 常用点

上下居中，给父盒子设置

```css
display: flex;
align-items: center; // 上下居中
```

![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071011.png)

左右居中

```
display:flex;
justify-content: center;
```

![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071010.png)

#### notice

今天在做一个模块的时候，发现 pc 端和移动端，或者这么说在 Chrome 和 Safari 上的展示效果有些区别，然后发现了一个新用法：

```css
flex: none;
```

**当你不希望自己的 dom 或者 el 被 flex 压缩时，加这个，非常好使**

既然都讲到了这里，顺便记录 2 行样式，用于多行省略使用，还是挺常用的：

```css
overflow: hidden;
text-overflow: ellipsis;
display: -webkit-box;
-webkit-box-orient: vertical;
-webkit-line-clamp: 2;  // 数字可变，2表示2行后 + ...
```
