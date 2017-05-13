---
layout: post
title: Django SQL debug工具
date: 2016-03-07
category: python
tag: django
---

**摘要:**
在django开发过程当中会涉及到sql语句的查看,第一个方法可以在shell里面print query
第二个方法是在console中log出执行的sql语句,第三个方法可以安装django-debug-toolbar.
查看什么时候执行了那些sql语句,有助于代码的优化和编码思路的提高.

##### 在setting中配置log

```
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'handlers': {
    'console':{
        'level':'DEBUG',
        'class':'logging.StreamHandler',
        },
    },
    'loggers': {
    'django.db.backends': {
        'handlers': ['console'],
        'propagate': True,
        'level':'DEBUG',
        },
    }
}
```


##### 安装配置django-debug-toolbar

[github](https://github.com/jazzband/django-debug-toolbar)
[django-debug-toolbar](https://django-debug-toolbar.readthedocs.io/en/stable/index.html)
