---
layout: post
title: python 一些工具
date: 2015-11-24
category: python
---
**摘要:**
Python中有很多的包可以使用,防止造轮子提高开发效率,降低bug数量.这里贴
一些链接和一些使用简介内容.


#### Gevent

- 协程不是进程或线程，其执行过程更类似于子例程，或者说不带返回值的函数调用
- 多个线程相对独立，有自己的上下文，切换受系统控制
- 协程相对独立，有自己的上下文，其切换由自己控制，由当前协程切换到其他协程由当前协程来控制。没有了线程所谓的安全问题
- 一个程序可以包含多个协程，可以对比与一个进程包含多个线程，因而下面我们来比较协程和线程
- gevent是第三方库，通过greenlet实现协程
    - greenlet 包是 Stackless 的副产品，其将微线程称为 “tasklet” 
    - tasklet运行在伪并发中，使用channel进行同步数据交换。一个”greenlet”，是一个更加原始的微线程的概念，但是没有调度，或者叫做协程。这在你需要控制你的代码时很有用。你可以自己构造微线程的 调度器；也可以使用”greenlet”实现高级的控制流。例如可以重新创建构造器；不同于Python的构造器，我们的构造器可以嵌套的调用函数，而被 嵌套的函数也可以 yield 一个值。Greenlet是作为一个C扩展模块给未修改的解释器的
    - 每个greenlet都只是heap(堆内存)中的一个python object(PyGreenlet).所以对于一个进程你创建百万甚至千万个greenlet都没有问题.
    - 每一个greenlet其实就是一个函数,以及保存这个函数执行时的上下文.对于函数来说上下文也就是其stack(栈).同一个进程的所有的greenlets共用一个共同的操作系统分配的用户栈.所以同一时刻只能有栈数据不冲突的greenlet使用这个全局的栈.
    - Simple case of greenlet  
    -  ```
    from greenlet import greenlet
    def test1():
        print 12
        gr2.switch()
        print 34
    def test2():
        print 56
        gr1.switch()
        print 78
    gr1 = greenlet(test1)
    gr2 = greenlet(test2)
    gr1.switch()
    ```

-  gevent 使用
1. 说明：
    - 使用gevent，可以获得极高的并发性能
    - gevent只能在Unix/Linux下运行
    - 在Windows下不保证正常安装和运行
    - 由于gevent是基于IO切换的协程，所以最神奇的是，我们编写的Web App代码，不需要引入gevent的包，也不需要改任何代码，仅仅在部署的时候，用一个支持gevent的WSGI服务器，立刻就获得了数倍的性能提升。

- 总结一下：
1. 多进程能够利用多核优势，但是进程间通信比较麻烦，另外，进程数目的增加会使性能下降，进程切换的成本较高。程序流程复杂度相对I/O多路复用要低。
2. I/O多路复用是在一个进程内部处理多个逻辑流程，不用进行进程切换，性能较高，另外流程间共享信息简单。但是无法利用多核优势，另外，程序流程被事件处理切割成一个个小块，程序比较复杂，难于理解。
3. 线程运行在一个进程内部，由操作系统调度，切换成本较低，另外，他们共享进程的虚拟地址空间，线程间共享信息简单。但是线程安全问题导致线程学习曲线陡峭，而且易出错。
4. 协程由编程语言提供，由程序员控制进行切换，所以没有线程安全问题，可以用来处理状态机，并发请求等。但是无法利用多核优势。
- **上面的四种方案可以配合使用，我比较看好的是进程+协程的模式。**

---

#### About Monkey Patch
- monkey patch指在运行时动态替换,一般是在startup的时候.
- 例如：gevent会在最开头的地方gevent.monkey.patch_all();把标准库中的thread/socket等给替换掉.这样我们在后面使用socket的时候可以跟平常一样使用,无需修改任何代码,但是它变成非阻塞的了.
- Example:
import json,后来发现ujson比自带json快了N倍,于是问题来了,难道几十个文件要一个个把import json改成import ujson as json吗?
其实只需要在进程startup的地方monkey patch就行了.是影响整个进程空间的.同一进程空间中一个module只会被运行一次.下面是代码.

`main.py`  

```
import json
import ujson
def monkey_patch_json():
  json.__name__ = 'ujson'
  json.dumps = ujson.dumps
  json.loads = ujson.loads

monkey_patch_json()
print 'main.py',json.__name__
import sub
```  

`sub.py`

```
import json
print 'sub.py',json.__name__
```
运行main.py,可以看到都是输出'ujson',说明后面import的json是被patch了的. 

**最后注意不能单纯的json = ujson来替换**

---

#### Scrapy
1. install
`sudo pip install scrapy`

2. Create A Project
`scrapy startproject tutorial`
  - 目录结构

```
tree tutorial/  
tutorial/  
├── scrapy.cfg  
└── tutorial  
    ├── __init__.py  
    ├── items.py  
    ├── pipelines.py  
    ├── settings.py  
    └── spiders  
        └── __init__.py  
```
- items.py: 定义项目的抓取条款准则（模型）　　
- spiders: 处理抓取实现　　
- piplines.py: 设计管道存储以及过滤抓取内容　　

3. shell中操作

`scrapy shell "http://www.dmoz.org/Computers/Programming/Languages/Python/Books/"`

[Doc Address](http://doc.scrapy.org/en/latest/intro/tutorial.html)
[Reference Link](http://scrapy-chs.readthedocs.org/zh_CN/latest/intro/tutorial.html)

---

#### spynner包支持ajax动态加载web浏览python包

- version: spynner 2.5 python >=26  PyQt > 443
- [Download](https://pypi.python.org/pypi/spynner/2.5)
- Spynner Requirement[x11](http://baike.baidu.com/link?url=8s5iBLGKeZiJ0moKSmV5QSJbX2hwK14vqvHF0oL3S7IvzJPFFU6CYg4MdeRpa71UkpgQ_vNVD9w4DJOFcPRoy_)


1. install x11  method :

- sudo apt-get install xorg-dev, libxtst-dev

- sudo pip install spynner

3. download the pyqt[pyqt](https://sourceforge.net/projects/pyqt/?source=typ_redirect)

4. sudo apt-get install libqt4-dev

5. sudo pip install PyQt

6. sudo pip install sip

**WARING**: Then run ipython or python with sudo if in vitrualevn source, OR you will be found the promote you PyQt4 not found!!!


---

#### pypi-server

`注意：`pip install pypiserver，　安装后会有报错　import sets as Set 的错误 

- 解决办法：　[Download](https://pypi.python.org/pypi/pypiserver#downloads)

- 安装 解压后的　requirements文件夹中的包

```
user1@localhost:~$ sudo pypi-server -p 7001 ~/simple/
serving on http://0.0.0.0:7001
```

---

#### pil histogram 统计的理解

```
    if the mode is RGB to open a image, 768 mean is Red grey list[0-255 pixs nums]+
    Green grey list[0-255 pix nums]+ Blue grey list[0-255 pix numbs]
    im is a Image object by RGB
    In [59]: x = reduce(lambda x,y:x+y,im.histogram())
    In [60]: x Out[60]: 47191725
    In [61]: im.size Out[61]: (4731, 3325)
    In [62]: 4731*3325 Out[62]: 15730575
    In [63]: 4731*3325*3 Out[63]: 47191725
    In [64]:
    or other way:
    r, g, b = im.split()
    len(r.histogram()) = 256
    len(g.histogram()) = 256
    len(b.histogram()) = 256
```

---

#### Install Kivy For Python GUI Aplication Develop
1. install cpython to support
   `sudo pip install cpython`
2. install pygame to support
   `sudo apt-get install python-pygame`
3. download the install file . [Adress](https://kivy.org/)
4. install kivy
   `sudo python setup.py install`
5. [Reference Link](http://inclem.net/pages/kivy-crash-course/)

---

#### Sklearn Install(Data Analysis)
[Reference Link](http://blog.sina.com.cn/s/blog_67331d610102vkvx.html)



---

#### Python Request

[Reference Link]http://docs.python-requests.org/zh_CN/latest/user/quickstart.html


---

#### Python Logging

[Reference Link](https://www.loggly.com/docs/django-logs/)



---

#### Flask Study

[Reference Link](http://www.pythondoc.com/flask-mega-tutorial/)

---

#### Django Celery

python manage.py celery worker --loglevel=info 


---

#### Excel handler

关于excel 模块

```
sudo pip install xlrd
sudo pip install xlwt
sudo pip install xlsxwriter
```


---
#### VirtualEnv python3 & Pip install pyton3 package 

1. `virtualenv -p python3 envname`
2. `python -m pip install <package-name>`
