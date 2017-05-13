---
layout: post
title: django on_commit
date: 2016-9-25
category: python
tag: django
---

**摘要:**
在orm提交之后执行一些操作,我们可以使用django 的on_commit.
[官网地址](https://docs.djangoproject.com/en/1.10/topics/db/transactions/#django.db.transaction.on_commit)
注意下New in Django 1.9.
有时候你需要安排一些操作与当前的数据库事务相关连,并且是在数据库事务成功完成
之后才能去执行相关的操作,比如说是一个异步任务,一个邮件提示,或者说是缓存过期处理等
因为你不想下一步处理的时候得到的是一个没有处理过的数据的情况发生.

- code
```
from django.db import transaction

def do_something():
    pass  # send a mail, invalidate a cache, fire off a Celery task, etc.
transaction.on_commit(do_something)

transaction.on_commit(lambda: some_celery_task.delay('arg1'))
```

- 如果不是一个有效的数据库事务则会立即执行
- 如果事务提交的时候出异常了也就是被回滚了,这个func也就会被废弃掉.

