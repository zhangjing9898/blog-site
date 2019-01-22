---
layout: post
title: "联动造成的blocking"
date: 2019-01-10
categories:
  - 踩坑记
tags:
  - 营销
description: 今天遇到了一个很神奇的问题，首先对问题进行一个复盘：
---

# 联动造成的blocking

今天遇到了一个很神奇的问题，首先对问题进行一个复盘：

#### 目的：

在影城卡和小食中，做到：在微信中展示一个引流的蒙层

#### 步骤：

我在`m-cards`和`snacks`中的changeCity fn中加入了一行:

```js
if (isInvalidEnv()) return;
```

接下来展示一下**isInvalidEnv的方法是怎么写的**

```js
// file1:
export const isInvalidEnv = isLockingAllJumpToPage('xxxx');

// file2:
import ClickBlocker from 'utils/click-bloker';
// 防500ms内的多次点击
const clickBlocker = new ClickBlocker(500);
// 重复点击、微信QQ
// microhead() -> 一个引流的蒙层
export const isLockingWXQQ = () => isBlocking() || microhead();

export const isLockingAllJumpToPage = (url) => {
  return () => {
    // 在开发环境 ignore
    if (BUILD_ENV.IS_DEV) return false;

    if (isLockingWXQQ()) return true;

    const isDev = window.location.href.indexOf('__dev__') > -1;
    if ((!UA.isTB && !UA.isAP && !UA.isDY) || isDev) {
      pushWindow(url);
      return true;
    }

    return false;
  };
};

// file3:


export default class ClickBlocker {

  constructor(period = 500) {
    this.period = period;
    this.timeMap = {};
  }

  isBlocking(methodName = 'default') {
    const now = new Date().getTime();
    const lastTime = this.timeMap[methodName];
    const isBlocking = lastTime && now - lastTime < this.period;
    this.timeMap[methodName] = now;

    return !!isBlocking;
  }

}

```

#### 结果：

咋一看，并没有什么妨碍，只是一个防多次点击 + 微信引流，为什么会引发：**卖品模块下，再次点击切换城市的时候，无法发送请求**

#### 原因：

因为影城卡和卖品目前是联动的，意思是，当你点击了切换城市，那么影城卡那的城市也会跟着变化，然后watcher中监听着`selectedCity`,一旦城市变化，就会跟着调用`changeCity()`这个方法,然后**ClickBlocker**中是检测的全局的。

也就是你点了卖品中的切换城市，同时 -> 影城卡联通，快速进入clickBlocker，系统会以为，**你多次对 切换城市 part进行点击，然后这时候isBlocking会为true -> isLockingAllJumpToPage为true**,
然后被return掉，无法执行下方的doFetch，然后导致无法发送请求。

