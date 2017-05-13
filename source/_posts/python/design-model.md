---
layout: post
title: 设计模式
date: 2016-2-17
category: 设计模式
---

**摘要:**
关于设计模式研究的很少只是网上看见了这个例子,这里摘抄下当做是以后学习设计模式的
引子吧,有些同事说设计模式在python中没有什么用处,有些书籍里面说设计模式在python
中很容易导致开发中代码混乱,没有起到好的作用反而导致一些问题.这里不表述这些,只是
记录下看看,说不定以后有用处.

##### 单例模式  

单例模式是一种常用的软件设计模式。在它的核心结构中只包含一个被称为单例类的特殊类
通过单例模式可以保证系统中该类只有一个实例而且该实例易于外界访问，从而方便对实例
个数的控制并节约系统资源。如果希望在系统中某个类的对象只能存在一个，单例模式是最
好的解决方案。Python有两种方式可以实现单例模式。


- 方式1

    ```
    class Singleton(type):
        def __init__(cls, name, bases, dict):
            super(Singleton, cls).__init__(name, bases, dict)
            cls.instance = None
     
        def __call__(cls, *args, **kw):
            if cls.instance is None:
                cls.instance = super(Singleton, cls).__call__(*args, **kw)
            return cls.instance
     
    class MyClass(object):
        __metaclass__ = Singleton
     
    print MyClass()
    print MyClass()
    ```
- 方式2

    使用decorator修饰器来实现单例模式。

    ```
    def singleton(cls):
        instances = {}
        def getinstance():
            if cls not in instances:
                instances[cls] = cls()
                return instances[cls]
            return getinstance
     
    @singleton
    class MyClass:
    …
    
    ```
