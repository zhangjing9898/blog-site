---
title: "Android下的1px兼容性问题"
date: 2018-12-26
categories:
  - 踩坑记
tags:
  - 兼容性 
  - Android
description: 今天在调试主会场页面的时候，发现在安卓真机下有一个常见的1px粗细不一致问题，也就是一个列表下，偶尔会出现一条线比其他的粗
---

# Android下的1px兼容性问题
 
 今天在调试主会场页面的时候，发现在安卓真机下有一个常见的1px粗细不一致问题，也就是一个列表下，偶尔会出现一条线比其他的粗，简单记录一下。

 目前网上有很多文章是讲`how to 解决移动端亚像素问题`,但是大多数方法都不能解决所有机型。

 比如
 
 🌰例子1，今天使用了 box-shadow的方法来解决。

 ```CSS
height: 1Px;
background: none;
box-shadow: 0 1px 0 #e1e1e1;
 ```

使用于iPhone6/7/8/x 和大部分安卓，但是在iPhone5/se下，线会消失

🌰例子2，还有常用的transform方法。

使用tranform放缩，根据after属性，绝对定位到元素上，将宽高放大2倍，然后用transform缩小0.5。

但是这个有可能在一些列表的情况下，导致某些线条不出来。

and so on ...

**废话不多说，直接上最终解决方法**

##### 1.是用背景图渐变实现的一像素边框

```CSS
.XXX(@color: @border-color, @bgColor: transparent) {
  height: 1px;
  ...
  background: @bgColor linear-gradient(to bottom, @color, @color 50%, transparent 50%, transparent 0) left top repeat-x;
  background-size: 100% 1px;
}
```

1px不明显，2px也OK滴

##### 2.是用before, after实现的一像素边框

```CSS
.hairline-all(@color: @border-color, @border-radius: 0) {
  position: relative;
  &::after {
    position: absolute;
    content: "";
    top: 0;
    left: 0;
    box-sizing: border-box;
    width: 100%;
    height: 100%;
    border: 1px solid @color;
    border-radius: @border-radius;
    pointer-events: none;
    -webkit-transform-origin: 0 0;
    transform-origin: 0 0;
  }
  @media (-webkit-min-device-pixel-ratio: 1.5),
  (min-device-pixel-ratio: 1.5),
  (min-resolution: 144dpi),
  (min-resolution: 1.5dppx) {
    &::after {
      width: 200%;
      height: 200%;
      -webkit-transform: scale(.5);
      transform: scale(.5);
    }
  }
  @media (-webkit-device-pixel-ratio: 1.5) {
    &::after {
      width: 150%;
      height: 150%;
      -webkit-transform: scale(.6666);
      transform: scale(.6666);
    }
  }
  @media (-webkit-device-pixel-ratio: 3) {
    &::after {
      width: 300%;
      height: 300%;
      -webkit-transform: scale(.3333);
      transform: scale(.3333);
    }
  }
}

// 通过before，after实现的1px边框-左边
.hairline-left(@color: @border-color, @top: 0, @bottom: 0) {
  position: relative;
  &::before {
    pointer-events: none;
    position: absolute;
    content: "";
    width: 1px;
    background: @color;
    top: @top;
    bottom: @bottom;
    left: 0;
    -webkit-transform-origin: 0 0;
    transform-origin: 0 0;
  }
  @media (-webkit-min-device-pixel-ratio: 1.5),
  (min-device-pixel-ratio: 1.5),
  (min-resolution: 144dpi),
  (min-resolution: 1.5dppx) {
    &::before {
      -webkit-transform: scaleX(.5);
      transform: scaleX(.5);
    }
  }
  @media (-webkit-device-pixel-ratio: 1.5) {
    &::before {
      -webkit-transform: scaleX(.6666);
      transform: scaleX(.6666);
    }
  }
  @media (-webkit-device-pixel-ratio: 3) {
    &::before {
      -webkit-transform: scaleX(.3333);
      transform: scaleX(.3333);
    }
  }
}
```

**补充一点** 

针对第二种伪类的解决方法，昨天在某安卓机型下出现了一个情况：

> 当我设置border的时候，一个长列表中，有概率出现某个盒子的border-bottom不出现的情况

这个情况怎么解决呢？

```css
.XXX{
  margin-bottom: 1px;
}# Android下的1px兼容性问题
 
 今天在调试主会场页面的时候，发现在安卓真机下有一个常见的1px粗细不一致问题，也就是一个列表下，偶尔会出现一条线比其他的粗，简单记录一下。

 目前网上有很多文章是讲`how to 解决移动端亚像素问题`,但是大多数方法都不能解决所有机型。

 比如
 
 🌰例子1，今天使用了 box-shadow的方法来解决。

 ```CSS
height: 1Px;
background: none;
box-shadow: 0 1px 0 #e1e1e1;
 ```

使用于iPhone6/7/8/x 和大部分安卓，但是在iPhone5/se下，线会消失

🌰例子2，还有常用的transform方法。

使用tranform放缩，根据after属性，绝对定位到元素上，将宽高放大2倍，然后用transform缩小0.5。

但是这个有可能在一些列表的情况下，导致某些线条不出来。

and so on ...

**废话不多说，直接上最终解决方法**

##### 1.是用背景图渐变实现的一像素边框

```CSS
.XXX(@color: @border-color, @bgColor: transparent) {
  height: 1px;
  ...
  background: @bgColor linear-gradient(to bottom, @color, @color 50%, transparent 50%, transparent 0) left top repeat-x;
  background-size: 100% 1px;
}
```

1px不明显，2px也OK滴

##### 2.是用before, after实现的一像素边框

```CSS
.hairline-all(@color: @border-color, @border-radius: 0) {
  position: relative;
  &::after {
    position: absolute;
    content: "";
    top: 0;
    left: 0;
    box-sizing: border-box;
    width: 100%;
    height: 100%;
    border: 1px solid @color;
    border-radius: @border-radius;
    pointer-events: none;
    -webkit-transform-origin: 0 0;
    transform-origin: 0 0;
  }
  @media (-webkit-min-device-pixel-ratio: 1.5),
  (min-device-pixel-ratio: 1.5),
  (min-resolution: 144dpi),
  (min-resolution: 1.5dppx) {
    &::after {
      width: 200%;
      height: 200%;
      -webkit-transform: scale(.5);
      transform: scale(.5);
    }
  }
  @media (-webkit-device-pixel-ratio: 1.5) {
    &::after {
      width: 150%;
      height: 150%;
      -webkit-transform: scale(.6666);
      transform: scale(.6666);
    }
  }
  @media (-webkit-device-pixel-ratio: 3) {
    &::after {
      width: 300%;
      height: 300%;
      -webkit-transform: scale(.3333);
      transform: scale(.3333);
    }
  }
}

// 通过before，after实现的1px边框-左边
.hairline-left(@color: @border-color, @top: 0, @bottom: 0) {
  position: relative;
  &::before {
    pointer-events: none;
    position: absolute;
    content: "";
    width: 1px;
    background: @color;
    top: @top;
    bottom: @bottom;
    left: 0;
    -webkit-transform-origin: 0 0;
    transform-origin: 0 0;
  }
  @media (-webkit-min-device-pixel-ratio: 1.5),
  (min-device-pixel-ratio: 1.5),
  (min-resolution: 144dpi),
  (min-resolution: 1.5dppx) {
    &::before {
      -webkit-transform: scaleX(.5);
      transform: scaleX(.5);
    }
  }
  @media (-webkit-device-pixel-ratio: 1.5) {
    &::before {
      -webkit-transform: scaleX(.6666);
      transform: scaleX(.6666);
    }
  }
  @media (-webkit-device-pixel-ratio: 3) {
    &::before {
      -webkit-transform: scaleX(.3333);
      transform: scaleX(.3333);
    }
  }
}
```

**补充一点** 

针对第二种伪类的解决方法，昨天在某安卓机型下出现了一个情况：

> 当我设置border的时候，一个长列表中，有概率出现某个盒子的border-bottom不出现的情况

这个情况怎么解决呢？

```css
.XXX{
  margin-bottom: 1px;
}
```

加一行边距
```

加一行边距