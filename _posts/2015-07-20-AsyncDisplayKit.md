---
layout: post
title: AsyncDisplayKit学习
---

{{ page.title }}
================

# 什么是AsyncDisplayKit

AsyncDispalyKit是大名鼎鼎的Facebook旗下的Paper团队发布的开源库。
目的是提高复杂UI的流畅性并缩短响应时间。

# AsyncDisplayKit的作用原理

我们知道，如果能保证每秒60帧的渲染频率，整个界面就会很流畅，给用户带来的体验就会非常好了。也就是说，
每一帧有1/60秒的时间，即不到16毫秒的时间去执行所有的布局和绘制。如果界面过于复杂，
在16毫秒之内完成不了要做的工作，就会导致丢帧，丢帧太多，就会出现界面卡顿的现象。

按照我们的常识，UI元素的显示和更新，都是在主线程上运算。如果把图像解码、布局以及渲染操作放在后台线程，
情况就会大大改观。

AsyncDisplayKit就是为此而生的。

AsyncDisplayKit使用 Display Node 层次结构替换视图层次结构和/或 Layer 树。各种 Display Node 是 AsyncDisplayKit 的关键所在。它们位于视图之上，而且是线程安全的，也就是说之前在主线程才能执行的任务现在也可以在非主线程执行。
这就能减轻主线程的工作量以执行其他操 作，例如处理触摸事件，或如在本应用的情况里，处理 Collection View 的滑动。

# 不使用AsyncDisplayKit时，会发生什么

请大家下载[初始项目](http://cdn1.raywenderlich.com/wp-content/uploads/2014/10/Layers-Start.zip)。
最好运行在iPad真机上，SDK支持8.1以上，而且越老的iPad越好。这就就能体验到UI卡顿是多么不能被接受的一件事了。

下面是项目的截图：

![a](../../../images/AsyncDisplayKit/1416797146276869.png)

[教程](http://www.raywenderlich.com/86365/AsyncDisplayKit-tutorial-achieving-60-fps-scrolling)中提到的
用Instruments检查帧率，将来我会单独写一篇博客讲解Instruments的使用。这里就不提了。

卡顿的原因就是在制作模糊背景时，需要做大量的计算，导致失帧。
#


# 参考资料

* [AsyncDisplayKit 官网](http://asyncdisplaykit.org/)
* [AsyncDisplayKit Github](https://github.com/facebook/AsyncDisplayKit)
* [AsyncDisplayKit 教程：达到 60 FPS 的滚动帧率](http://www.cocoachina.com/swift/20141124/10298.html)
* [AsyncDisplayKit Tutorial: Achieving 60 FPS scrolling](http://www.raywenderlich.com/86365/AsyncDisplayKit-tutorial-achieving-60-fps-scrolling)
* [AsyncDisplayKit Tutorial: Node Hierarchies](http://www.raywenderlich.com/107310/asyncdisplaykit-tutorial-node-hierarchies)
* [AsyncDisplayKit技术分析](http://xujim.github.io/ios/2014/12/07/AsyncDisplayKit_inside.html)
