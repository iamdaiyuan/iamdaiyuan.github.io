---
layout: post
title: Oracle导入数据时发生的错误总结
---

{{ page.title }}
================

用Oracle数据库导入数据，用下面的命令就可以完成

```sql
imp <username>/<password>@<dbname> file=\usr\local\<dump_file_name.dmp> ignore=y 
```

# 问题一

问题描述：not a valid export file,header failed verification.

原因：dmp文件是从orcale 11g导出的，要导入到orcale 10g数据库中，版本不匹配，导致此问题的出现。

解决办法：用文本编辑器打开.dmp文件，修改版本号，将11.00.01修改为10.00.01即可

# 问题二

问题描述：在导入过程中，有一个警告信息：Warning: the objects were exported by <xxx>, not by you.
xxx是导出数据库的那个用户名。

原因：导出导入的用户不是同一个用户，所以报错。

解决办法：在命令行中，加入fromuser,touser参数。最终，命令行是：


```sql
imp <username>/<password>@<dbname> fromuser=<xxx> touser=<yyy> 
file=\usr\local\<dump_file_name.dmp> ignore=y 
```
