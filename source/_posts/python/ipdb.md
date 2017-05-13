---
layout: post
title: ipdb 常用命令
date: 2016-01-014
category: python
tag: debug
---
**摘要**
程序开发的过程中少不了需要debug场景,Python可以使用pdb或者ipd来调试,
ipdb需要自己安装下包,其中tab使用非常方便,这里笔记下一些常用的调试
命令.


#### ipdb调试常用命令

1. python -m ipdb xxx.py 逐行调试
2. 断点调试  

```
import ipdb
ipdb.set_trace()
```

#### 命令行

- h, 帮助
- l <行号>, 查看代码指定行开始的后11行,行号默认为当前行
- p <变量>, 打印变量
- ENTER, 重复上次命令
- q, 退出
- n, 执行下一句
- c, 继续执行
- s, 进入子程序
- r, 继续执行，直到函数体return语句
- !<python 命令>, 执行python命令
- a, 打印当前函数的参数
- j, 让程序跳转到当前代码块的指定行
- w, 当前执行位置

[推荐链接](http://blog.csdn.net/lengyisheng/article/details/18254763)

