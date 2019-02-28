---
title: "egret"
date: 2019-2-28
categories:
  - egret
  - 游戏引擎
description: egret入门
---

最近，空下来准备研究一下用egret来做动画和游戏。

# egret是什么？
Egret是一套HTML5游戏开发解决方案，产品包含Egret Engine，Egret Wing，EgretVS，Res Depot，Texture Merger，TS Conversion，Egret Feather，Egret Inspector，DragonBones，Lakeshore等。而核心产品是Egret Engine，是一个基于TypeScript语言开发的一个HTML5游戏引擎，其余的大多是开发和辅助工具。

# 安装 + 初步调试

参考[官网](https://egret.com/)  step by step进行安装 + 初步调试

# Hello World

**项目创建好之后，我们进入index.html**
然后找到对应的data-show-fps和data-show-log，然后将其的值改为true，方便我们观察到游戏运行的实时帧率
![image.png](https://upload-images.jianshu.io/upload_images/3378252-ea4d7e12874844fc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

设置完之后，run后会是这样滴~

![image.png](https://upload-images.jianshu.io/upload_images/3378252-5a13c68e5fd79ac6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**接下来，我们来进行Hello World的入门**

找到src下的main.ts

再进入游戏创建场景的函数 createGameScene

加上这么一段：

```js
       // 创建一个文字对象
        let text = new egret.TextField();
        // set xy 
        text.x = 100;
        text.y = 100;
        // set color 
        text.textColor = 0x888888;
        // set container 
        text.text = 'Hello world~ dididi'
        // add to Main 
        this.addChild(text);
```

然后点击`项目 >  构建`，构建成功后，点击`项目 > 调试`

这时候，我们的hello world dididi 就跑出来啦~

![image.png](https://upload-images.jianshu.io/upload_images/3378252-676495ed03bbd81c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

嗯... 是不是很简单，接下来继续

## 启动

```CLI
xxx(project name)
egret create xxx
egret build xxx
egret startserver xxx -a
```

## draw
1.draw 这里参数描述了当前画面渲染时候drawcall的次数
2.cost 包含四个参数 enterframe阶段的开销
引擎updateTransform开销
引擎draw开销
html5中canvas draw的开销
3 fps 当前画面的帧屏

x y scalex和y alpha 透明度 rotation旋转

0x+颜色

bitmap 位图
shape 矢量图
textfield
textinput 文本

## 遮罩 mask

shape绘制图形
xxx.mask

## 检测碰撞

hitTestPoint(x, y, true)
return boolean
true是看 与图形是否重合

## 锚点

anchorOffsetX anchorOffsetY位置

## 添加与删除显示对象

sprite 算是一个轻量级的显示对象

添加也就是 a.addChild(b)
删除也就是 a.removeChild(b)

在删除的时候，需要注意一点，如果a不是this，而是一个容器或者其他的，那么我们需要先判断它是否存在，如果存在才能进行删除操作。
```
if(a.parent）{
 a.removeChild(b)
}
```
## 深度管理

深度管理就像是一个队列，但是更像是z-序列号这种东西。

Egret中容器的深度都是从0开始的，当一个显示对象第一个被添加到容器中时，它的深度值为0。这个显示对象也处于容器的最底层。当我们添加第二个显示对象的时候，他的深度值为1，并且在第一个显示对象上方。如果两个显示对象发生了相交，那么我们可以从视觉上看到，第二个显示对象遮挡住第一个显示对象。

### 深度插队

当我们想讲某一个显示对象添加到一个指定深度的时候，我们需要使用 `addChildAt` 方法。这个操作很像排队时插队的想象。

使用 `addChildAt` 方法也非常的容易，具体使用方法如下：

`容器.addChildAt( 显示对象, 深度值 )`

### 交换深度

交换不同对象深度的功能Egret为开发者提供了两个方法。一个是 `swapChildren` 方法，另外一个是 `swapChildrenAt` 方法。

两个方法使用方式少有不同，但效果相同，具体使用方法如下：

`容器.swapChildren( 显示对象, 显示对象 )`

`容器.swapChildrenAt( 深度值, 深度值 )`


### 重设子对象深度

当我们将一个显示对象添加到显示列表中后，我们还可以手动重设这个显示对象的深度。

实现显示对象深度重置的方法是 `setChildIndex` ，使用方法如下：

`容器.setChildIndex( 显示对象, 新的深度值 );`

