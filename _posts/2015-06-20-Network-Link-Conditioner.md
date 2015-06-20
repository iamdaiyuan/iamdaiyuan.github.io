---
layout: post
title: 网络状况模拟工具：Network Link Conditioner
---

{{ page.title }}
================

Network Link Conditioner 是苹果公司提供的一款精确模拟网络环境的工具。
可以方便开发者模拟用户在不同网络环境下的使用体验。

## 下载安装

* 登陆[苹果开发者官网](https://developer.apple.com/downloads/index.action?q=Network%20Link%20Conditioner)
* 搜索：hardware io tools，找到对应的xcode版本，点击下载

![a](../../../images/NetworkLinkConditioner/1.png)

* 双击“Network Link Conditioner.prefPane”，安装即可。

![a](../../../images/NetworkLinkConditioner/2.png)

* 安装完成后，会再“系统偏好设置”中，加入该程序的设置项，界面如下图。

![a](../../../images/NetworkLinkConditioner/3.png)

你可以模拟各类网络情况：

```
EDGE
3G
DSL
WiFi
High Latency DNS
Very Bad Network
100% Loss
```

## 后记

工欲善其事，必先利其器。在APP越来越同质化的今天，把握细节，把产品做到极致，离不开这些工具的支持。