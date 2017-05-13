---
layout: post
title: Python Object的继承差异
date: 2015-01-07
category: python
tag: python
---

**摘要:**
在Python经常使用的class继承问题并没有弄的清楚,这里做一些记录备忘.
在Python2.2之后引入了继承object的新式类,在Python3里面这些得到了统一即使不去继承object也是一样的新式类的效果.

##### Python2.7
1. 不继承object
```
>>> class A:
...     pass
... 
>>> type(A)
<type 'classobj'>
>>> dir(A)
['__doc__', '__module__']
>>> a = A()
>>> type(a)
<type 'instance'>
>>> dir(a)
['__doc__', '__module__']
>>> 

```
2. 继承object
```
>>> class B(object):
...     pass
... 
>>> type(B)
<type 'type'>
>>> dir(B)
['__class__', '__delattr__', '__dict__', '__doc__', '__format__', '__getattribute__', '__hash__', '__init__', '__module__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__']
>>> b = B()
>>> type(b)
<class '__main__.B'>
>>> dir(b)
['__class__', '__delattr__', '__dict__', '__doc__', '__format__', '__getattribute__', '__hash__', '__init__', '__module__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__']
>>> 

```


##### Python3.5
1. 不继承object
```
>>> class A:
...     pass
... 
>>> type(A)
<class 'type'>
>>> dir(A)
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__']
>>> 
>>> a = A()
>>> type(a)
<class '__main__.A'>
>>> dir(a)
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__']
>>> 
```
2. 继承object
```
>>> class B(object):
...     pass
... 
>>> type(B)
<class 'type'>
>>> dir(B)
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__']
>>> 
>>> b = B()
>>> type(b)
<class '__main__.B'>
>>> dir(b)
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__']
>>> 
```

