---
title: "发送2次index.html"
date: 2018-12-27
categories:
  - 踩坑记
tags:
  - HTML
description: 今天遇到一个很神奇的问题，打开一个页面，会发送2次index.html。
---



# 发送2次index.html

今天遇到一个很神奇的问题，打开一个页面，会发送2次index.html。
分别来自这2个地方：
- 一次type:document,initiator:other
- 一次来自appear.js

🍉很奇怪对吧，正常来说，只会只会有一个html的，细查之后发现，问题出在这里。

```css
.xxx{
   background: url('') top;
}
```
因为url为空，所以又去请求了一次主页....

验证：
造一个假url，比如：url('http://xxxx.jpg')，即可验证