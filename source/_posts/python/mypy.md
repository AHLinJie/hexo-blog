---
layout: post
title: python代码静态检查
date: 2016-06-22
category: python
tag: mypy
---
**摘要:**
众所周知,Python是一门动态语言,具体解释我就不说了,动态语言有个问题是不做类型检查.
在自己开发或者持续集成过程使用mypy可以降低程序的bug数量,代码更健壮,有时候可以提高
代码的阅读效果.


#### 代码静态检查顺序

默认情况mypy先检查定义的函数和返回，然后检查函数内调用的函数定义和返回，最后检查引用包。


1. mypy --py2  manage.py flashsale/coup  # 指定目录检查（python2）

- [网址](http://mypy-lang.org/)
- [GitHub](https://github.com/python/mypy)  
- [参考文档](http://mypy.readthedocs.io/en/latest/python2.html)

