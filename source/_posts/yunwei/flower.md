---
layout: post
title: Flower
date: 2016-07-16
category: 运维
tag: celery
---
**摘要:**

在使用celery task 异步任务的时候可以使用这个monitor来观察task的运行情况.
同样可以是用flower来调试运行的task,查看程序报错栈信息,很方便.

#### Celery Monitor: Flower

`$ pip install flower`

`$ celery -A shopmanager flower`
- [文档](http://flower.readthedocs.io/en/latest/)
- [Download](http://flower.readthedocs.io/en/latest/)
- [参考链接](http://my.oschina.net/u/2306127/blog/420929)


