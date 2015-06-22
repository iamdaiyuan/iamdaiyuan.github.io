---
layout: post
title: 实现一个波浪动画
---

{{ page.title }}
================

感谢[董铂然](https://github.com/dsxNiubility)的分享。
我们先直观的看下效果图：

![a](../../../images/waveAnimation/waveAnimate.gif)

## 原理

原理比较简单，用一张大的有波浪的图片，从视图的左下角移动到右上角，达到波浪涌动的效果。

就是这张图：

![a](../../../images/waveAnimation/fb_wave.png)

核心的代码如下：

``` objective-c
CGFloat avgScore = self.precent;
[UIView animateWithDuration:4.0 animations:^{
    self.bigImg.top = 115 - ((avgScore/100.0) * 115);
    if (avgScore == 100) {
        self.bigImg.top = -20;
    }

    self.bigImg.left = 0;
}];
```
