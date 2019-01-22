---
layout: post
title: "vue-scrollToTop"
date: 2019-01-09
categories:
  - vue
tags:
  - vue
description: 今天涉及，在 vue 中做一个回到顶部效果的动画，简单几行就可以做到，记录一下。
---

# vue-scrollToTop

今天涉及，在 vue 中做一个回到顶部效果的动画，简单几行就可以做到，记录一下。

fn 函数:

```js
      // element：dom元素
      // to：滚动到哪个位置
      // duration: 动画时长
      scrollToTop(element, to, duration) {
        if (duration <= 0) return;
        const diff = to - element.scrollTop;
        const perTick = diff / duration * 10;
        this.timer = setTimeout(() => {
          element.scrollTop += perTick;
          if (element.scrollTop === to) return;
          this.scrollToTop(element, to, duration - 10);
        }, 10);
      }
```

如何使用：

```JS
xxx() {
  this.scrollToTop(xxx, 0, 300);
}
// 声明
data() {
  return {
    timer: null
  };
}
// 销毁timer
beforeDestroy() {
  // clear timer
  clearInterval(this.timer);
}
```
