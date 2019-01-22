---
layout: post
title: "charles使用规则"
date: 2019-01-12
categories:
  - 踩坑记
tags:
  - 代理
description: Charles是一款代理服务器
---

# charles使用规则

Charles是一款代理服务器，通过过将自己设置成系统（电脑或者浏览器）的网络访问代理服务器，然后截取请求和请求结果达到分析抓包的目的。

在平时开发中进行使用，用于模拟各种异常情况。

## Charles的主要功能：

- 截取Http 和 Https 网络封包。

- 支持重发网络请求，方便后端调试。

- 支持修改网络请求参数。

- 支持网络请求的截获并动态修改 map local

- 模拟慢速网络 && breaking point

## 安装配置步骤

具体的安装配置步骤可以参考这几篇文章:

https://www.jianshu.com/p/fb2bdde5b498

https://www.cnblogs.com/xingzc/p/7896924.html

或者自行google

## 注意点

我在使用Charles的时候，之前遇到了几个蠢蠢的问题，📝记录一下：

- 每个模拟器，都需要install对应的证书certificate，如果你的模拟器重装了一次，请记住**证书也需要重装一次**，一个模拟器就相当于一个终端，和你在手机上代理是一样的。

#### 如何安装对应的certificate？

![image.png](https://upload-images.jianshu.io/upload_images/3378252-1da2700db5dd4937.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

证书安装成功后，我们就可以方便的抓模拟器上的包，这时候，需要注意一点：

`主客上是这样的:`
![image.png](https://upload-images.jianshu.io/upload_images/3378252-3243ff1762e042cf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然鹅，`钱包上由于域名收敛的问题，它是长这样的:`
![image.png](https://upload-images.jianshu.io/upload_images/3378252-6b3c673d6a055426.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



### 如何map local？

map local是我们前端自测的一个很重要的方法来模拟各种异常，怎么操作呢？很简单，2步搞定👌。

1. 找到对应的请求，然后我们先把request result保存下来，名字自定义，建议：xxx.json(xxx和request相关)

![image.png](https://upload-images.jianshu.io/upload_images/3378252-78fe009c3ba73057.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. copy request的url

![](https://upload-images.jianshu.io/upload_images/3378252-c63be864e38a7df5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3. 进入map local

![image.png](https://upload-images.jianshu.io/upload_images/3378252-37982c531a28072a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4. add
   
   - click -> add,然后进入这个页面
  ![image.png](https://upload-images.jianshu.io/upload_images/3378252-8d9210fabd96aea8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  - 把上一步复制的url，pause到host框里面
  - 光标点击**Path**输入框，这时候，它会自动填补
  - 将**Query**框中的所有值**重置**为 * ；
  - 这时候map from就设置好了，接下来设置map to
  - 在**Local path**中choose对应，我们需要模拟的接口response即可


