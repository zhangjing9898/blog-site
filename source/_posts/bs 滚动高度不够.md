---
layout: post
title: "better-scroll 滚动高度不够问题"
date: 2019-01-02
categories:
  - 踩坑记
tags:
  - plugin
description: w前几天，在做我的奖品列表模块的时候，使用了 better-scroll，发现出现一个问题：有时候会出现内容页的滚动高度不够
---
# better-scroll 滚动高度不够问题

前几天，在做我的奖品列表模块的时候，使用了 better-scroll，发现出现一个问题：**有时候会出现内容页的滚动高度不够**。

😅 最后发现，是因为每一个 li 的高度不一致，导致，better-scroll 在初始化时图片没有加载完，并且服务端下发的图片的高度不一定，无法计算出最终需要**translate 的高度**。

当时有点蠢没有反应过来，想用一个 fn 来计算 dom 的高度。

🌰 比如：

```JS
updated () {
        //解决better-scroll因为图片没有下载完导致的滚动条高度不够，无法浏览全部内容的问题。
        //原因是better-scroll初始化是在dom加载后执行，此时图片没有下载完成，导致滚动条高度计算不准确。
        //利用图片的complete属性进行判断，当所有图片下载完成后再对scroll重新计算。
        let img = document.getElementsByClassName('content')[0].getElementsByTagName('img')
        let count = 0
        let length = img.length
        if (length) {
            let timer = setInterval(() => {
                if (count == length) {
                    // console.log('refresh')
                    this.scroll.refresh()
                    clearInterval(timer)
                } else if (img[count].complete) {
                    count ++
                }
            }, 100)
        }

    },
```

#### 但是，完全不必要这样。

本身li的高度就应该固定住，让img的width自缩放，不然，不同高度的li，这样的列表看起来一点都不美观。