---
title: "egret"
date: 2019-2-28
categories:
  - egret
  - 游戏引擎
description: egret入门
---

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

### 事件处理机制的原理

event dispatcher

#### 事件流程
egret 事件机制包括4个步骤：

-  注册侦听器
- 发送事件
- 侦听事件
- 移除侦听器

按照上方面顺序**依次执行**

注册侦听器使用事件发送者的`addEventListener()`将相应的事件分配给侦听器

```js
public addEventListener(type:string, listener:Function, thisObject:any, useCapture:boolean = false, priority:number = 0)
```

*   type：事件类型，必选。
*   listener：用来处理事件的侦听器，必选。
*   thisObject：作用域，必选，一般填写this。因为TypeScript与JavaScript的this作用域不同，其this指向也会不同。如果不填写this的话，那么编译后的代码会发生错误。 关于this的问题，可以学习JavaScript中的原型链。
*   useCapture: 确定侦听器是运行于捕获阶段还是运行于冒泡阶段，可选。设置为 true，则侦听器只在捕获阶段处理事件，而不在冒泡阶段处理事件。设置为 false，则侦听器只在冒泡阶段处理事件。
*   priority： 事件侦听器的优先级，可选。优先级由一个带符号的整数指定。数字越大，优先级越高。优先级为 n 的所有侦听器会在优先级为 n -1 的侦听器之前得到处理。如果两个或更多个侦听器共享相同的优先级，则按照它们的添加顺序进行处理。默认优先级为 0

#### 触摸事件

Egret中有专门的触摸事件类，使用触摸事件时，默认需要打开显示对象的触摸开关，即将`touchEnabled`设置为`true`。






