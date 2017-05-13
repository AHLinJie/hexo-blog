---
layout: post 
title: python基础
date: 2015-01-04
category: python
tag: python
---

**摘要:**
Python是一门语法非常优雅的语言,很多人都是这样说,我也就这么肯定了,开发过程中一直
使用的就是python.大部分计算机或者是人类语言入门都很简单,只要花点时间学习下就可以,
但是想要精通一门语言到大师的水准还是很难的,哪怕是python这样一门业内公认的比较
容易的语言.路漫漫其修远兮,吾将上下而求索!


#### 拷贝一个对象（赋值，浅拷贝，深拷贝的区别）
1. 赋值（=）: 就是创建了对象的一个新的引用，修改其中任意一个变量都会影响到另一个。
2. 浅拷贝：创建一个新的对象，但它包含的是对原始对象中包含项的引用（如果用引用的方式修改其中一个对象，另外一个也会修改）
    2.1 完全切片方法；
    2.2 工厂函数，如list()；
    2.3 copy模块的copy()函数}
3. 深拷贝：创建一个新的对象，并且递归的复制它所包含的对象（修改其中一个，另外一个不会改变）
    3.1 copy模块的deep.deepcopy()函数

#### Python随机数

1. 随机整数：random.randint(a,b)：返回随机整数x,a<=x<=b
random.randrange(start,stop,[,step])：返回一个范围在(start,stop,step)之间的随机整数，不包括结束值。
2. 随机实数：random.random():返回0到1之间的浮点数
random.uniform(a,b):返回指定范围内的浮点数。


#### 单引号，双引号，三引号的区别

单引号和双引号是等效的，如果要换行，需要符号(\),三引号则可以直接换行，并且可以包含注释
单引号：s4 = ‘Let\’s go’
双引号：s5 = “Let’s go”
s6 = ‘I realy like“python”!’

#### range()函数用法

如果需要迭代一个数字序列的话，可以使用range()函数，range()函数可以生成等差级数。
如例：
```
    range(10)               # 输出结果：[0,1,2,3,4,5,6,7,8,9]
    range(5, 10)            # 从5开始，输出结果：[5,6,7,8,9]
    range(0, 10, 3)         # 增量为3，输出结果：[0,3,6,9]
    range(-10, -100, -30)   # 负增长，输出结果：[-10, -40, -70]
    # 可以一起使用range()和len()来迭代一个索引序列
    a = ['Nina', 'Jim', 'Rainman', 'Hello']
    for i in range(len(a)):
        print(i, a[i])
```
#### lambda函数
Python允许定义单行的小函数。
lambda函数默认返回表达式的值。你也可以将其赋值给一个变量。lambda函数可以接受任意个参数，包括可选参数，但是表达式只有一个。
如果函数非常简单，只有一个表达式，不包含命令，可以考虑lambda函数。否则，定义函数。
```
g = lambda x, y: x*y
g(3,4)
g = lambda x, y=0, z=0: x+y+z
(lambda x,y=0,z=0:x+y+z)(3,5,6)  # 也能够直接使用lambda函数，不把它赋值给变量
```

#### 类型转换
Python提供了将变量或值从一种类型转换成另一种类型的内置函数。int函数能够将符合数学格式数字型字符串转换成整数。否则，返回错误信息。
- 整型转换
    int(”34″)
    int(”1234ab”)  # 不能转换成整数
    int(34.1234)
    int(-2.46)
- 函数float将整数和字符串转换成浮点数
    float(”12″)
    float(”1.111111″)
- 函数str将数字转换成字符
    str(98)
    ‘98’
    str(”76.765″)
    ‘76.765′
    **整数1和浮点数1.0在python中是不同的。虽然它们的值相等的，但却属于不同的类型。这两个数在计算机的存储形式也是不一样。**

#### pass语句的作用是

- pass语句什么也不做，一般作为占位符或作者创建占位程序，pass语句不会执行任何操作，比如：
```
while False:
    pass
```
- pass通常用来创建一个最简单的类：
```
class MyEmptyClass:
    pass
```
- pass在软件设计阶段也经常用来作为TODO，提醒实现相应的实现.
```
def initlog(*args):
    pass  # todo: please implement this
```
#### 定义函数

1. 函数的定义
```
def <name>(arg1, arg2,… argN):
    <statements>
```
函数的名字也必须以字母开头，可以包括下划线,但不能把Python的关键字定义成函数的名字。函数内的语句数量是任意的，每个语句至少有一个空格的缩进，以表示此语句属于这个函数的。缩进结束的地方，函数自然结束。函数的目的是把一些复杂的操作隐藏，来简化程序的结构，使其容易阅读。函数在调用前，必须先定义。也可以在一个函数内部定义函数，内部函数只有在外部函数调用时才能够被执行。程序调用函数时，转到函数内部执行函数内部的语句，函数执行完毕后，返回到它离开程序的地方，执行程序的下一条语句。

2. function里面设置全局的变量

解决方法是在function的开始插入一个global声明：
```
def f()
    global x
```

#### 进行内存管理

Python的内存管理是由Python解释器负责的，开发人员可以从内存管理事务中解放出来，致力于应用程序的开发，这样就使得开发的程序错误更少，程序更健壮，开发周期更短。从三个方面来说,一.对象的引用计数机制,二.垃圾回收机制,三.内存池机制。

1. 对象的引用计数机制
python内部使用引用计数，来保持追踪内存中的对象，所有对象都有引用计数。
    1.1 引用计数增加的情况：
    一个对象分配一个新名称
    将其放入一个容器中（如列表、元组或字典）
    1.2 引用计数减少的情况：
    使用del语句对对象别名显示的销毁
    1.3 引用超出作用域或被重新赋值:
    sys.getrefcount()函数可以获得对象的当前引用计数
    多数情况下，引用计数比你猜测得要大得多。对于不可变数据（如数字和字符串），解释器会在程序的不同部分共享内存，以便节约内存。
2. 垃圾回收
当一个对象的引用计数归零时，它将被垃圾收集机制处理掉。
当两个对象a和b相互引用时，del语句可以减少a和b的引用计数，并销毁用于引用底层对象的名称。然而由于每个对象都包含一个对其他对象的应用，因此引用计数不会归零，对象也不会销毁。（从而导致内存泄露）。为解决这一问题，解释器会定期执行一个循环检测器，搜索不可访问对象的循环并删除它们。

3. 内存池机制
Python提供了对内存的垃圾收集机制，但是它将不用的内存放到内存池而不是返回给操作系统。
Pymalloc机制。为了加速Python的执行效率，Python引入了一个内存池机制，用于管理对小块内存的申请和释放。
Python中所有小于256个字节的对象都使用pymalloc实现的分配器，而大的对象则使用系统的malloc。
***对于Python对象，如整数，浮点数和List，都有其独立的私有内存池，对象间不共享他们的内存池。也就是说如果你分配又释放了大量的整数，用于缓存这些整数的内存就不能再分配给浮点数。***

#### 反序迭代序列

- 如果是一个list, 最快的解决方案是
```
list.reverse()
```
- 如果不是list, 最通用但是稍慢的解决方案是
```
for i in range(len(sequence)-1, -1, -1):
    x = sequence[i]
    print x
    <do something with x>
```
#### tuple和list的转换

函数tuple(seq)可以把所有可迭代的(iterable)序列转换成一个tuple, 元素不变，排序也不变。例如，tuple([1,2,3])返回(1,2,3), tuple(’abc’)返回(’a’，’b',’c').如果参数已经是一个tuple的话，函数不做任何拷贝而直接返回原来的对象，所以在不确定对象是不是tuple的时候来调用tuple()函数也不是很耗费的。函数list(seq)可以把所有的序列和可迭代的对象转换成一个list,元素不变，排序也不变。例如 list([1,2,3])返回(1,2,3), list(’abc’)返回['a', 'b', 'c']。如果参数是一个list, 她会像set[:]一样做一个拷贝



## 构建描述符对象

描述符可以改变其他对象，也可以是访问类中任一的getting,setting,deleting。描述符不意味着孤立；相反，它们意味着会被它们的所有者类控制。当建立面向对象数据库或那些拥有相互依赖的属性的类时，描述符是有用的。当描述符在几个不同单元或描述计算属性时显得更为有用。作为一个描述符，一个类必须至少实现__get__,__set__,和__delete__中的一个。

- __get__(self, instance, owner)
当描述符的值被取回时定义其行为。instance是owner对象的一个实例，owner是所有类。
- __set__(self, instance, value)
当描述符的值被改变时定义其行为。instance是owner对象的一个实例，value是设置的描述符的值
- __delete__(self, instance)
当描述符的值被删除时定义其行为。instance是owner对象的一个实例。

- 单位转换中描述符使用
```
class Meter(object):
    '''描述符'''
    def __init__(self, value=0.0):
        self.value = float(value)
    def __get__(self, instance, owner):
        return self.value
    def __set__(self, instance, value):
        self.value = float(value)

class Foot(object):
    '''英尺描述符'''
    def __get__(self, instance, owner):
        return instance.meter * 3.2808
    def __set__(self, instance, value):
        instance.meter = float(value) / 3.2808

class Distance(object):
    '''表示距离的类，控制2个描述符：feet和meters'''
    meter = Meter()
    foot = Foot()
```

#### Pickling对象

Pickling是一种对Python数据结构的序列化过程。如果你需要存储一个对象，之后再取回它（通常是为了缓存）那么它就显得格外地有用了。有一个词典你想要保存它并在稍后取回。你可以把它的内容写到一个文件中去，需要非常小心地确保你写了正确的语法，然后用exec()或处理文件的输入取回写入的内容。但这是不稳定的:如果你你在纯文本中保存重要的数据，它有可能被几种方法改变，导致你的程序crash或在你的计算机上运行了恶意代码而出错。pickling并非完美。Pickle文件很容易因意外或出于故意行为而被损毁。Pickling可能比起使用纯文本文件安全些，但它仍旧有可能会被用来跑恶意代码。还有因为Python版本的不兼容问题，所以不要期望发布Pickled对象，也不要期望人们能够打开它们。但是，它依然是一个强大的缓存工具和其他常见序列化任务。

1. pickle
```
import pickle
data = {'foo': [1,2,3],
        'bar': ('Hello','world!'),
        'baz': True}
jar = open('data.pk1', 'wb')
pickle.dump(data, jar)  # 把pickled数据写入jar文件
jar.close()
```

2. unpickle
```
import pickle
pk1_file = open('data.pk1','rb') #连接pickled数据
data = pickle.load(pk1_file) #把数据load到一个变量中去
print data
pk1_file.close()
```

3. Pickling你自定义的对象
Pickling不仅可用在内建类型上，还可以用于遵守pickle协议的任何类。pickle协议有4个可选方法用于定制Python对象如何运行.
    3.1 __getinitargs__(self)
        如果你想当你的类unpickled时调用__init__，那你可以定义__getinitargs__，该方法应该返回一个元组的参数，然后你可以把他传递给__init__。注意，该方法仅适用于旧式类。
    3.2 __getnewargs__(self)
        对于新式类，你可以影响有哪些参数会被传递到__new__进行unpickling。该方法同样应该返回一个元组参数，然后能传递给__new__
    3.3 __getstate__(self)
        代替对象的__dict__属性被保存。当对象pickled，你可返回一个自定义的状态被保存。当对象unpickled时，这个状态将会被__setstate__使用。
    3.4 __setstate__(self, state)
        对象unpickled时，如果__setstate__定义对象状态会传递来代替直接用对象的__dict__属性。这正好跟__getstate__手牵手：当二者都被定义了，你可以描述对象的pickled状态，任何你想要的。


4. 例子：
Slate类，它会记忆它曾经的值和已经写入的值。然而，当这特殊的slate每一次pickle都会被清空,当前值不会被保存。
```
import time
class Slate:
    '''存储一个字符串和一个变更log，当Pickle时会忘记它的值'''

    def __init__(self, value):
        self.value = value
        self.last_change = time.asctime()
        self.history = {}

    def change(self, new_value):
        # 改变值，提交最后的值到历史记录
        self.history[self.last_change] = self.value
        self.value = new_value
        self.last_change = time.asctime()

    def print_changes(self):
        print 'Changelog for Slate object:'
        for k, v in self.history.items():
            print '%st %s' % (k, v)

    def __getstate__(self):
        # 故意不返回self.value 或 self.last_change.
        # 当unpickle，我们希望有一块空白的"slate"
        return self.history

    def __setstate__(self, state):
        # 让 self.history = state 和 last_change 和 value被定义
        self.history = state
        self.value, self.last_change = None, None
```


#### 团队开发中调用接口

Python可以实现多继承,没有接口,Java中存在接口.如果真要说接口调用可以用通过类封装好方法,然后让其他子类继承后实例化后调用.

#### 上下文管理

在 Python2.5里引入了一个新关键字（with）使得一个新方法得到了代码复用。上下文管理这个概念在Python中早已不是新鲜事了（之前它作为库 的一部分被实现过），但直到[PEP343](http://www.python.org/dev/peps/pep-0343/)才作为第一个类语言结构,取得了重要地位而被接受。上下文管理允许对对象进行设置和清理动作，用with声明进行已经封装的操作。上下文操作的行为取决于2个方法：
1. __enter__(self)
定义块用with声明创建出来时上下文管理应该在块开始做什么。注意，__enter__的返回值必须绑定with声明的目标，或是as后面的名称。
2. __exit__(self,  exception_type, exception_value, traceback)
定义在块执行（或终止）之后上下文管理应该做什么。它可以用来处理异常，进行清理，或行动处于块之后某些总是被立即处理的事。如果块执行成功的话，exception_type，exception_value，和traceback将会置None。否则，你可以选择去处理异常，或者让用户自己去处理。确保在全部都完成之后__exit__会返回True。如果不想让上下文管理处理异常，那就让它发生好了。
__enter__和__exit__对那些已有良好定义和对设置，清理行为有共同行为的特殊类是有用。你也可以使用这些方法去创建封装其他对象通用的上下文管理。
```
class Closer:
    """用with声明一个上下文管理, 用一个close方法自动关闭一个对象
    """
    def __init__(self, obj):
        self.obj = obj

    def __enter__(self):
        return self.obj  # 绑定目标

    def __exit__(self, exception_type, exception_val, trace):
        try:
            self.obj.close()
        except AttributeError:  # obj不具备close
            print 'Not closable.'
            return True  # 成功处理异常
```



#### 接口和抽象类（ABC:abstract base class）

1. 抽象基类
    有些面向对象的语言,如JAVA，支持接口，可以声明一个支持给定的一些方法，或者支持给定存取协议的类。抽象基类（或者ABCs）是Python里一个相同的特性。抽象基类由abc模块构成，包含了一个叫做ABCMeta的metaclass。这个metaclass由内置的isinstance()和issubclass()特别处理，并包含一批会被 Python开发人员广泛用到的基础抽象基类。Python 2.6的 collections模块包括了许多不同的抽象基类来表示出这些不同。Iterable表明一个类定义了__iter__()，Container意味着该类定义了__contains__()方法，因此支持x in y表达式。基本的dictionary接口包括存取数据和 keys()，values()，以及items()，由MutableMapping抽象基类定义。你可以让你的类继承某个特定的抽象基类，来表示它们支持抽象基类接口：
    ```
    import collections
    class Storage(collections.MutableMapping):
        pass
    ```
    另外，你可以不继承基类，以调用抽象基类的register()方法的方式注册该类,使得该类支持抽象基类接口。
    
    ```
    import collections
    class Storage:
        pass
    collections.MutableMapping.register(Storage)
    ```

    1.1 相对于你写的类来说，从抽象基类继承可能更清晰。当你已经写了一个新的抽象基类，能描述一个存在的类型或类，或者你想声明某些第三方类来实现一个抽象基类，register()方法是有用的。例如，如果你定义了一个PrintableType抽象基类，以下是合法的:

    ``` 
        PrintableType.register(int)
    
        PrintableType.register(float)
    
        PrintableType.register(str)
    ```
    1.2 类应当遵循由抽象基类指明的语义，但是Python不能检查这一点；类作者应该理解抽象基类的需求，并据此实现代码。要检查一个对象是否支持某个特定接口，你可以这样写：
    
    ```
    def func(d):
      if not isinstance(d, collections.MutableMapping):
        raise ValueError("Mapping object expected, not %r" % d)
    ```
    不要认为你必须像上面的例子那样，写许多检查性的代码。Python有很强的duck-typing传统，从来不会进行显式的类型检查。代码只是简单的调用对象的方法，认为这些方法会存在，如果不存在就会抛出异常。抽象基类检查时一定要谨慎，最好在非常必要的时候才那样做。

    1.3 你可以在类的定义中用abc.ABCMeta作为metaclass写自己的抽象基类：
    ```
    from abc import ABCMeta, abstractmethod
    class Drawable():

        __metaclass__ = ABCMeta
        @abstractmethod
        def draw(self, x, y, scale=1.0):
            pass

        def draw_doubled(self, x, y):
            self.draw(x, y, scale=2.0)

    class Square(Drawable):
        def draw(self, x, y, scale):
            pass
    ```
    在以上的Drawable抽象基类中，draw_doubled()方法会按对象两倍的大小画出来，并且可以调用Drawable自己的方法来实现。因此实现这个抽象基类的类不需要提供它们自己的draw_doubled()实现，尽管他们可以那样做。然而draw()的实现是必须的；抽象基类不能提供一个有用的一般实现。你可以在必须实现的方法，如draw()中应用@abstractmethod修饰符；Python会对那些没有定义该方法的类抛出异常。注意，只有当你试图创建一个子类实例，但是却缺少该方法的时候才会抛出异常。
    ```
    class Circle(Drawable):
        pass

    c=Circle()
    Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    TypeError: Can't instantiate abstract class Circle with abstract methods draw
    ```

    1.4 抽象数据属性可以用@abstractproperty decorator声明：
    ```
    from abc import abstractproperty

    @abstractproperty
    def readonly(self):
        return self._x
    ```
    子类必须定义一个readonly()属性。

注意：句柄是指使用的一个唯一的整数值，即一个4字节(64位程序中为8字节)长的数值，来标识应用程序中的不同对象和同类对象中的不同的实例，诸如，一个窗口，按钮，图标，滚动条，输出设备，控件或者文件等。应用程序能够通过句柄访问相应的对象的信息，但是句柄不是一个指针，程序不能利用句柄来直接阅读文件中的信息。如果句柄不在I/O文件中，它是毫无用处的。


#### import机制

1. 当import一个module时，假设我们执行,import hello 过程如下：
    1.1 搜索sys.modules['hello']是否存在，
        ```
            if getattr(sys.modules, 'hello') is none：
                sys.modules['hello'] = hello
                执行hello中的语句
            else：
                pass
        ```
    1.2 globals()[‘hello'] = hello
    每当我们使用一个module时，会从globals()中寻找，如果没有发现就报NameError。也就是说一个对象（python万物皆对象）能用，它必须满足两个条件： globals()包含该module。

2. 当我们执行from mylib import hello过程如下：
    ```
    if  getattr(sys.modules, 'mylib.hello') is none:
         sys.modules['mylib'] = mylib
         sys.modules['mylib.hello'] = hello
         # 已经该mylib下的所有对象全都加载到sys.modules中。
         # 执行mylib.__init__.py中语句
         # 执行mylib.hello.py中语句（注意该mylib下的其它module不会在执行）
    globals()['hello'] = hello
    ```
当一个对象被import时，如果sys.modules中没有该对象值，那么会执行它的所包含的语句以完成初始化。反之就不会执行它的语句。为什么对象已存在于sys.modules中，却不能用。因为使用一个对象时，是搜索globals()，如何有就可用。sys.modules只能说明该对象是否加载，不能说明已经在当前环境的全局变量中。sys.modules可以将对象存放在内存中，加快访问速度。




#### 闭包

闭包的特点是返回的函数还引用了外层函数的局部变量，所以，要正确使用闭包，就要确保引用的局部变量在函数返回后不能变。
```
def count():
    fs = []
    for i in range(1, 4):
        def f(j):
            def g():
                return j*j
            return g  # 正确地返回一个闭包g，g所引用的变量j不是循环变量，因此将正常执行  形参的作用体现 i -> j
        r=f(i)
        fs.append(r)
    return fs

f1, f2, f3 = count()
print f1(), f2(), f3()
```

#### 装饰器
1. 编写无参数decorator
Python的 decorator 本质上就是一个高阶函数，它接收一个函数作为参数，然后，返回一个新函数。

```
import time

def performance(f):
    def fn(*args,**kw):  # *args 和 **kw，保证任意个数的参数总是能正常调用
        t1 = time.time()
        r = f(*args,**kw)
        t2 = time.time()
        print 'call %s() in %fs'%(f.__name__,(t2-t1))
        return r
    return fn

@performance
def factorial(n):
    return reduce(lambda x,y: x*y, range(1, n+1))

print factorial(10)

```

2. 编写带参数decorator－@performance 增加一个参数，允许传入's'或'ms'

```
import time
def performance(unit):
    def perf_decorator(f):
        def wrapper(*args, **kw):
            t1 = time.time()
            r = f(*args, **kw)
            t2 = time.time()
            t = (t2 - t1) * 1000 if unit=='ms' else (t2 - t1)
            print 'call %s() in %f %s' % (f.__name__, t, unit)
            return r
        return wrapper
    return perf_decorator
@performance('ms')
def factorial(n):
    return reduce(lambda x,y: x*y, range(1, n+1))
print factorial(10)
```


----

## 基础操作

#### 文件操作

- 删除文件
使用os.remove(filename)或者os.unlink(filename)。
```
import os
os.remove(‘kk.html’)
os.unlink(‘kk.html’)
```
- copy文件

shutil模块有一个copyfile函数可以实现文件拷贝。
```
import shutil
shutil.copyfile('kk.html', 'kkk.html')
```

#### 发送邮件

可以使用smtplib标准库。以下代码可以在**支持SMTP监听器**的服务器上执行。
```
import sys, smtplib
fromaddr = raw_input(“FROM：”)
toaddrs = raw_input(”To: “).split(’,')
print “Enter message, end with ^D:”
msg = ”
while 1:
    line = sys.stdin.readline()
    if not line:
        break
        msg = msg + line
# 发送邮件部分
server = smtplib.SMTP(’localhost’)
server.sendmail(fromaddr, toaddrs, msg)
server.quit()
```

----

## 工具选择

#### python bug和代码分析工具

- 有，PyChecker是一个python代码的静态分析工具，它可以帮助查找python代码的bug, 会对代码的复杂度和格式提出警告.
- Pylint是另外一个工具可以进行coding standard检查。

---

## 字符串处理

#### 正则表达式

- 正则贪婪匹配和非贪婪匹配
    当重复匹配一个正则表达式时候， 例如<.*>, 当程序执行匹配的时候，会返回最大的匹配值
    ```
    import re
    s = ‘<html><head><title>Title</title>’
    print(re.match(’<.*>’, s).group())
    ```
    会返回一个匹配<html><head><title>Title</title>而不是<html>;而
    ```
    import re
    s = ‘<html><head><title>Title</title>’
    print(re.match(’<.*?>’, s).group())
    ```
    则会返回<html>
    <.*>这种匹配称作贪心匹配 <.*?>称作非贪心匹配


- search()和match()的区别
    - match（）函数检测RE是不是在string的开始位置匹配， search()会扫描整个string查找匹配, match（）只有在0位置匹配成功的话才有返回，如果不是开始位置匹配成功的话，match()就返回none。
        ```
        print(re.match(’super’, ’superstition’).span())会返回(0, 5)
        print(re.match(’super’, ‘insuperable’))则返回None
        ```
    - search()会扫描整个字符串并返回第一个成功的匹配
        ```
        print(re.search(’super’, ’superstition’).span())返回(0, 5)
        print(re.search(’super’, ‘insuperable’).span())返回(2, 7)
        ```

#### 查询和替换一个文本字符串

1. 可以使用sub()方法来进行查询和替换，sub方法的格式为：
    sub(replacement, string[, count=0])
    replacement是被替换成的文本
    string是需要被替换的文本
    count是一个可选参数，指最大被替换的数量
    ```
        import re
        p = re.compile(’(blue|white|red)’)
        print(p.sub(’colour’,'blue socks and red shoes’))
        print(p.sub(’colour’,'blue socks and red shoes’, count=1))
    ```
2. subn()方法执行的效果跟sub()一样，不过它会返回一个二维数组，包括替换后的新的字符串和总共替换的数量
    ```
        import re
        p = re.compile(’(blue|white|red)’)
        print(p.subn(’colour’,'blue socks and red shoes’))
        print(p.subn(’colour’,'blue socks and red shoes’, count=1))
    ```

## 异常处理
#### except的用法和作用
Python的except用来捕获所有异常， Python错误都会抛出一个异常，每个程序的错误都被当作一个运行时错误,语法形式：`try…except…except…[else…][finally…]`。执行try下的语句，如果引发异常，则执行过程会跳到except语句。对每个except分支顺序尝试执行，如果引发的异常与except中的异常组匹配，执行相应的语句。如果所有的except都不匹配，则异常会传递到下一个调用本代码的最高层try代码中。try下的语句正常执行，则执行else块代码。如果发生异常，就不会执行。如果存在finally语句，最后总是会执行。


#### python程序中文输出问题

1. encode和decode
```
    import os.path
    import xlrd,sys
    Filename=’/home/tom/Desktop/1234.xls’
    if not os.path.isfile(Filename):
        raise NameError,”%s is not a valid filename”%Filename
    bk=xlrd.open_workbook(Filename)
    shxrange=range(bk.nsheets)
    print shxrange
    for x in shxrange:
        p=bk.sheets()[x].name.encode(‘utf-8′)
        print p.decode(‘utf-8′)
```

2. 在文件开头加上
```
reload(sys)
sys.setdefaultencoding(‘utf8′)
```
    2.1 如何获得系统的默认编码
    ```
        #!/usr/bin/env python
        #coding=utf-8
        import sys
        print sys.getdefaultencoding()
    ```

#### python操作数据库

1. 修改表字段名
```
import MySQLdb
try:
    conn=MySQLdb.connect(host='localhost',user='root',passwd='root',db='test',port=3306)
    cur=conn.cursor()
    cur.execute('select * from user')
    cur.close()
    conn.close()
except MySQLdb.Error,e:
    print "Mysql Error %d: %s" % (e.args[0], e.args[1])
```

2. 插入数据，批量插入数据，更新数据
    ```
    import MySQLdb

    try:
        conn=MySQLdb.connect(host='localhost',user='root',passwd='root',port=3306)
        cur=conn.cursor()

        cur.execute('create database if not exists python')
        conn.select_db('python')
        cur.execute('create table test(id int,info varchar(20))')

        value=[1,'hi rollen']
        cur.execute('insert into test values(%s,%s)',value)

        values=[]
        for i in range(20):
            values.append((i,'hi rollen'+str(i)))
            cur.executemany('insert into test values(%s,%s)',values)
            cur.execute('update test set info="I am rollen" where id=3')

        conn.commit()
        cur.close()
        conn.close()

    except MySQLdb.Error,e:
        print "Mysql Error %d: %s" % (e.args[0], e.args[1])
    ```
    **注意一定要有conn.commit()这句来提交事务，要不然不能真正的插入数据。**

    ```
    import MySQLdb

    try:
        conn=MySQLdb.connect(host='localhost',user='root',passwd='root',port=3306)
        cur=conn.cursor()
        conn.select_db('python')
        count=cur.execute('select * from test')
        print 'there has %s rows record' % count

        result=cur.fetchone()
        print result
        print 'ID: %s info %s' % result

        results=cur.fetchmany(5)
        for r in results:
            print r
        print '=='*10
        cur.scroll(0,mode='absolute')
        results=cur.fetchall()
        for r in results:
            print r[1]
        conn.commit()
        cur.close()
        conn.close()

    except MySQLdb.Error,e:
        print "Mysql Error %d: %s" % (e.args[0], e.args[1])
    ```
3. 修改表列名称
    ```
    try:
        conn = MySQLdb.connect(host='localhost', user='root', passwd='linjie520', port=3306)
        cur = conn.cursor()
        cur.execute('CREATE DATABASE IF NOT EXISTS mytest3 CHARACTER SET UTF8')
        conn.select_db('mytest3')
        cur.execute('CREATE TABLE test(id int,info varchar(20))')
        cur.execute('ALTER TABLE test ADD age INT UNSIGNED NOT NULL DEFAULT 10')
        cur.execute('ALTER TABLE test CHANGE age money INT UNSIGNED NOT NULL DEFAULT 1000')

        value = [1, 'hello world1', 1200]
        cur.execute('INSERT INTO TEST VALUES(%s,%s,%s)', value)
        conn.commit()
        count = cur.execute('SELECT * FROM test')
        print count
        results = cur.fetchmany()
        for r in results:
            print(r)
        cur.close()
        conn.close()
        cur.scroll(0, mode='absolute')
        results = cur.fetchall()
        for r in results:
            print(r[1], r[2])
    except MySQLdb.Error, e:
        print "MySQLdb Error %d :%s", (e.args[0], e.args[1])
    ```
查询后中文会正确显示，但在数据库中却是乱码的。处理方法：
charset是要跟你数据库的编码一样，如果是数据库是gb2312 ,则写charset='gb2312'。
`conn = MySQLdb.Connect(host='localhost', user='root', passwd='root', db='python') 中加一个属性：`
改为：
`conn = MySQLdb.Connect(host='localhost', user='root', passwd='root', db='python', charset='utf8')`


4. 常用的函数：
    连接对象也提供了对事务操作的支持,标准的方法  
    4.1 commit() 提交  
    4.1 rollback() 回滚  
    4.3 cursor用来执行命令的方法  
    4.4 callproc(self, procname,args):用来执行存储过程,接收的参数为存储过程名和参数列表,返回值为受影响的行数  
    4.5 execute(self, query,args):执行单条sql语句,接收的参数为sql语句本身和使用的参数列表,返回值为受影响的行数  
    4.6 executemany(self, query, args):执行多条sql语句,但是重复执行参数列表里的参数,返回值为受影响的行数  
    4.7 nextset(self):移动到下一个结果集  
    4.8 cursor用来接收返回值的方法  
    4.9 fetchall(self):接收全部的返回结果行.  
    4.10 fetchmany(self, size=None):接收size条返回结果行.如果size的值大于返回的结果行的数量,则会返回cursor.arraysize条数据.  
    4.11 fetchone(self):返回一条结果行.  
    4.12 scroll(self, value, mode='relative'):移动指针到某一行.如果mode='relative',则表示从当前所在行移动value条,如果 mode='absolute',则表示从结果集的第一行移动value条.



## python socket编程

#### 编写server的步骤:
1. 创建socket对象。调用socket构造函数。
socket = socket.socket(family, type )
family参数代表地址家族，可为AF_INET或AF_UNIX。AF_INET家族包括Internet地址，AF_UNIX家族用于同一台机器上的进程间通信。
type参数代表套接字类型，可为SOCK_STREAM(流套接字)和SOCK_DGRAM(数据报套接字)。
2. 将socket绑定到指定地址。这是通过socket对象的bind方法来实现的：
socket.bind( address )由AF_INET所创建的套接字，address地址必须是一个双元素元组，格式是(host,port)。host代表主机，port代表端口号。如果端口号正在使用、主机名不正确或端口已被保留，bind方法将引发socket.error异常。
3. 使用socket套接字的listen方法接收连接请求。
socket.listen( backlog )
backlog指定最多允许多少个客户连接到服务器。它的值至少为1。收到连接请求后，这些请求需要排队，如果队列满，就拒绝请求。
4. 服务器套接字通过socket的accept方法等待客户请求一个连接。
connection, address =socket.accept()
调用accept方法时，socket会时入“waiting”状态。客户请求连接时，方法建立连接并返回服务器。accept方法返回一个含有两个元素 的元组(connection,address)。第一个元素connection是新的socket对象，服务器必须通过它与客户通信；第二个元素 address是客户的Internet地址。
5. 处理阶段，服务器和客户端通过send和recv方法通信(传输数据)。服务器调用send，并采用字符串形式向客户发送信息。send方 法返回已发送的字符个数。服务器使用recv方法从客户接收信息。调用recv 时，服务器必须指定一个整数，它对应于可通过本次方法调用来接收的最大数 据量。recv方法在接收数据时会进入“blocked”状态，最后返回一个字符串，用它表示收到的数据。如果发送的数据量超过了recv所允许的，数据 会被截短。多余的数据将缓冲于接收端。以后调用recv时，多余的数据会从缓冲区删除(以及自上次调用recv以来，客户可能发送的其它任何数据)。
6. 传输结束，服务器调用socket的close方法关闭连接。

```
# server.py

if __name__ =='__main__':
    import socket
    sock = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
    sock.bind(('localhost',8001))
    sock.listen(5)
while True:
    connection,address =sock.accept()
try:
    connection.settimeout(5)
    buf =connection.recv(1024)
    if buf == '1':
        connection.send('welcometo server!')
    else:
        connection.send('please go out!'
        except socket.timeout:
        print 'time out'
    connection.close()
```

#### python编写client的步骤:
1. 创建一个socket以连接服务器：socket= socket.socket( family, type )
2. 使用socket的connect方法连接服务器。对于AF_INET家族,连接格式如下：
socket.connect((host,port) )
host代表服务器主机名或IP，port代表服务器进程所绑定的端口号。如连接成功，客户就可通过套接字与服务器通信，如果连接失败，会引发socket.error异常。
3. 处理阶段，客户和服务器将通过send方法和recv方法通信。
4. 传输结束，客户通过调用socket的close方法关闭连接。
```
# client.py

if __name__ =='__main__':
    import socket
    sock =socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.connect(('localhost',8001))
    import time
    time.sleep(2)
    sock.send('1')
    print sock.recv(1024)
    sock.close()
```
5. 在终端运行server.py，然后运行clien.py，会在终端打印“welcome to server!"。如果更改client.py的sock.



#### python自动连接ssh的代码

```
#!/usr/bin/python

import sys, time, os

try:
    import pexpect
except ImportError:
    print """You must install pexpect module"""
    sys.exit(1)

addr_map = {
    'v3' :('root@192.168.1.162', 'sina@2009'),
    'dev':('test016@192.168.1.136', 'test016'),
}

try:
    key = sys.argv[1]
    host = addr_map[key]
except:
    print """argv error, use it link ssh v3, v3 must defined in addr_map"""
    sys.exit(1)
    server = pexpect.spawn('/usr/bin/ssh %s' % host[0])
    server.expect('.*ssword:')
    server.sendline(host[1])
    server.interact()
```


#### 把原函数的所有必要属性都一个一个复制到新函数上，所以Python内置的functools可以用来自动化完成这个“复制”的任务


```
import time, functools
def performance(unit):
    def perf_decorator(f):
        @functools.wraps(f)
        def wrapper(*args, **kw):
            t1 = time.time()
            r = f(*args, **kw)
            t2 = time.time()
            t = (t2 - t1) * 1000 if unit=='ms' else (t2 - t1)
            print 'call %s() in %f %s' % (f.__name__, t, unit)
            return r
        return wrapper
    return perf_decorator
@performance('ms')
def factorial(n):
    return reduce(lambda x,y: x*y, range(1, n+1))
print factorial.__name__
```

#### python 反射机制
1. 通过类名获得该类的实例对象
2. 通过方法名得到方法对象

##### person.py
```
#coding=utf-8

class Peson(object):
    
    def __init__(self, name):
        self.name = name

    def get_name(self):
        return self.name


def say_hello():
    print 'hello'
```
##### 调用实现
1. 使用globals实现反射
```
from person import Person
p = globals()['Person']('linjie')
p.get_name()
```
2. 使用__import__, getattr实现反射
同理可以使用getattr、hasattr、delattr和setattr来实现完整的反射内容
```
module = __import__('person')
p = getattr(module, 'Person')('linjie')
p.get_name()
```
3. 反射调用函数
```
import person
func = getattr(person, 'say_hello')  # getattr返回的是一个对象
func()
```


#### 目录扫描
```
import os
import sys

  
def scan_files(directory,prefix=None,postfix=None):
  files_list=[]

  for root, sub_dirs, files in os.walk(directory):
    for special_file in files:
      if postfix:
        if special_file.endswith(postfix):
          files_list.append(os.path.join(root,special_file))
      elif prefix:
        if special_file.startswith(prefix):
          files_list.append(os.path.join(root,special_file))
      else:
        files_list.append(os.path.join(root,special_file))  
  return files_list

if __name__ == '__main__':
    directory = sys.argv[1]
    l = scan_files(directory)
    print l
```
