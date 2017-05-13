---
layout: post
title: django 信号
date: 2015-06-10
category: python
tag: django
---

**摘要:**
django中提供了信号机制,可以很好的起到解耦的作用这里笔记一两点使用过程中需要
注意的地方, 比如执行顺序啦.建议在编写信号的代码的时候,写到model下面,这样便于
阅读代码,使得代码容易codereview,并且在团队开发的过程中更容易发现,以免不必要的
麻烦.因为之前有写过在别的地方connect了一个model的信号.导致debug过程中很难找到
原因.还有就是尽量减少使用signal的使用.因为很多情况下发出的信号不知道有哪些
function去connect了.一定要知道自己在干什么.
如果一定要使用的话尽量减少connect.

- 代码演示

```
from django.db.models.signals import post_save
import time

def bbb(sender, instance, created, raw, **kwargs):
    print('call bbb')

def aaa(sender, instance, created, raw, **kwargs):
    print('call aaa')
    time.sleep(3)
    print('sleep done')

post_save.connect(aaa, sender=ModelName, dispatch_uid='post_save_bbb')
post_save.connect(bbb, sender=ModelName, dispatch_uid='post_save_aaa')

```
- 执行结果
```
(u'call aaa')
sleep done
(u'call bbb')
```
- 使用receivers来查看信号接收的function
```
In [5]: from django.db.models.signals import post_save
In [6]: post_save.receivers
[((u'post_save_bbb', 38420912),
          <weakref at 0x7f62fb927998; to 'function' at 0x7f62fb8c7848 (aaa)>),
     ((u'post_save_aaa', 38420912),
        <weakref at 0x7f62fb9279f0; to 'function' at 0x7f62fb8c77d0 (bbb)>)]
```
**注意:**这里我故意将dispatch_uid给调换了

- 总结

信号的执行顺序是按照connect的先后执行的.
