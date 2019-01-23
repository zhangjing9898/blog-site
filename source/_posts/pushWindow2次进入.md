---
layout: post
title: "pushWindow跳转2次页面问题"
date: 2019-01-21
categories:
  - 踩坑记
tags:
  - 营销
description: 今天遇到一个问题，用AlipayJSBridge中pushwindow的时候，如果url为主客的链接，那么页面会进入2次。
---

# pushWindow跳转2次页面问题

今天遇到一个问题，用AlipayJSBridge中pushwindow的时候，如果url为主客的链接，那么页面会进入2次。

客户端那边最初给出的代码是这样：

```js
window.AlipayJSBridge.call('pushWindow', {
  url: 'tbmovie://taobao.com/h5jumpurl=https%3a%2f%2fwww.taobao.com%3fenableWK%3dYES',
})
```
然后我们这边和大麦的同学，都发现这些写，会跳转2次页面，如何解决呢？

之后我采用bridge中封了一层的pushWindow来写，也就是:

```js
import { getClientInfo, pushWindow } from '@ali/tbm-bridge';

pushWindow('tbmovie://taobao.com/h5jumpurl=https%3a%2f%2fwww.taobao.com%3fenableWK%3dYES')
```

❓为什么这样就解决了呢？

- 因为tbmovie alipays协议这种 自带就有pushwindow的效果。像tbmovie这样的schema用location.href，其他正常的就是AlipayJSBridge中的pushWindow
