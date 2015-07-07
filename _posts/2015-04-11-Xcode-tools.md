---
layout: post
title: 开发工具、技巧总结
---

{{ page.title }}
================

# Xcode代码片段创建和导入工具

手工创建Xcode的片段，大部分人都会。但是不是略感繁琐？而且不便于分享。[mattt大师](https://github.com/mattt)为我们开发了一个好工具，用来导入代码片段。

## 片段导入工具xcodesnippet

安装片段导入工具

``` ruby
$ gem install xcodesnippet
```

## 片段的编写

``` c
---
title: "Hello, World!"
summary: "Prints 'Hello World'"
completion-scope: Function or Method
---

println("Hello, World!")
```

## 片段的导入

```
$ xcodesnippet install path/to/source.m
```

## 代码片段的资源和分享

[Xcode-Snippets](https://github.com/Xcode-Snippets/Objective-C)
