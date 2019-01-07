---
title: "Safari日期相关坑"
date: 2018-12-21
categories:
  - 踩坑记
tags:
  - 兼容性 
  - Safari
description: Safari日期相关坑
---

# Safari日期坑

今天在改卖品和影城卡部分，发现一个坑，如果设置endDate为24:00，并用`new Date(xx/xx/xx 24:00)`在Safari下会出现一个value invalid的问题，因为没有24:00这个写法，在电子表的显示中都是23.59-> 0:00，但是在Chrome下，会有一个容错。

在Chrome下，它会直接将24:00转成下一天的0:00。

所以注意在Safari下，我们需要做一个容错的处理，比如最简单粗暴的方法。

```JS
if(endTime === '24:00'){
  // 手动赋值
  // 很粗暴...的方法
  xxx.endTime = '23:59'
}
```