---
layout: post
title: python内建函数
date: 2015-04-01
category: python
tag: python
---
**摘要:**
python中和其他语言一样有很多的内建函数,也就是一些已经定要的预定义函数,方便开发
人员在开发中方便的使用这些函数提高开发效率.比如一些数学函数,字符串函数等等.


abs(x)  
返回绝对值，x可以使普通类型，长整型，浮点型；如果是复数类型则返回复数的模。
注意：在python中复数表示方法为：a+bj  等同于数学中的：a+b*i

all(iterable)

如果iterable(可迭代的参数)是中的所有element是真则返回真，否则返回假。

运行：
all([1,2,3])
all((1,2,3,0))

any(iterable)
如果iterable(可迭代的参数)是中的存在element是真则返回真，否则返回假。

运行：
any([1,2,3])
any((1,2,3,0))

basestring()
是str和unicode的超类（父类），也是抽象类，因此不能被调用和实例化，但可以被用来判断一个对象是否为str或者unicode的实例，isinstance(obj, basestring)等价于isinstance(obj, (str, unicode))；

运行：
isinstance("Hello world", str)
isinstance("Hello world", basestring)
isinstance(u"你好", unicode)
isinstance(u"你好", basestring)
bin(x)
将integer转换为对应的二进制的字符串，如果参数不是integer，就必须重写一个__index__()方法，并且这个方法返回一个integer类型值。
运行：
bin(1)
bin('a')  # 报错

bool([x])
将数值x转换为Boolean类型，如果是FALSE或者缺省则返回FALSE，否则返回真bool也是一个类，是int的子类。

运行：
bool()
bool(1) 
bool('a') 
bool((1,2,3,0)) 
bool((1,2,3,0))

bytearray([source[, encoding[, errors]]])
返回一个新的字节数组，bytearray是一个可变序列的类型，序列元素取值区间为[0,255]。
如果参数是一个字符串，方法按照encoding将字符串转换为字节序列；
如果参数是一个integer，方法创建integer长度的空字节序列；
如果参数是一个符合buffer接口类型的对象，方法将只读缓冲区对象将用于初始化的字节数组；
如果参数是可迭代的，则必须是[0,255]之间的integer，用于初始化数组的内容。

运行：
a = bytearray(3)
a
bytearray(b'\x00\x00\x00')
a[0]
a[1] 
a[2] 
b = bytearray("abc") 
b
b[0]
b[1] 
b[2] 
c = bytearray([1,2,3]) 
c
c[0]
c[1] 
c[2] 
d = bytearray(buffer("abc")) 
d
d[0]
d[1] 
d[2] 
e = bytearray()
e

callable(object)
如果参数对象呈现的可以调用则返回True，否则返回False;注意返回为True并不意味着可以调用，返回为False则一定不可以调用；类可以调用，而类实例在没有__call__()方法时候是不可以调用的。

运行：
callable(0)
callable('abc') 
def add(a,b): 
    return a+b
callable(add) 
class A: 
    pass
callable(A) 
a = A() 
callable(a)
class B:
    def __call__(self):
         pass
callable(B) 
b=B() 
callable(b)

chr(i)
将对应i数值的ASCII码返回，参数i的区间为[0,255],超出区间则会ValueError；有相反作用的函数式ord(c)。

运行：
chr(1)
chr(98) 
chr(98) 
ord('b') 
chr(266)
Traceback (most recent call last):
  File "<pyshell#25>", line 1, in <module>
    chr(266)
ValueError: chr() arg not in range(256)

classmethod(function)
一个类方法接收类隐式的第一个参数,就像一个实例方法接收实例。
classmethod是用来指定一个类的方法为类方法，没有此参数指定的类的方法为实例方法，使用方法如下：

class C:
    @classmethod        # 装饰器形式调用
        def f(cls, arg1, arg2, ...): ...
注意：实例方法是建立实例才有的方法，类方法是直接可以使用类引用，类方法既可以直接类调用(C.f())，也可以进行实例调用(C().f())。一般在项目中类方法都是设置为工具类使用的。
运行：
```
>>> class C:
	@classmethod
	def f(self):
		print 'this is a class method'
>>> C.f()
    # 直接调用
this is a class method
>>> c = C()
    # 实例化
>>> c.f()
        # 实例调用
this is a class method

>>> class D:
	def f(self):
		print 'this is not a class method'
>>> D.f()        # 调用失败
Traceback (most recent call last):
  File "<pyshell#55>", line 1, in <module>
    D.f()
TypeError: unbound method f() must be called with D instance as first argument (got nothing instead)
>>> d=D()
        # 实例化
>>> d.f()
        # 实例调用
this is not a class method
```

cmp(x, y)
比较x与y的值，如果x>y则返回1，x<y返回-1,x=y返回0.

运行：
cmp(1,2)
cmp(2,1)
cmp(3,3)

compile(source, filename, mode[, flags[, dont_inherit]])
将source编译为代码或者是AST（abstract syntax tree）对象。代码可以通过exec命令执行，或者可以通过调用eval()函数来处理。


