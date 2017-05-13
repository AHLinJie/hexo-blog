---
layout: post
title: actions 示例
date: 2016-11-03
category: python
tag: test
---

**摘要:**
编写项目测试代码的时候会遇到很多问题,比如测试环境的搭建,测试数据库的准备,以及冗余
测试数据准备不仅导致开发时间的延长,部署难度的增加,甚至导致了代码的混乱难以维护.

以下是刁老师给的一个解决方案.

##### 目录结构

├── actions.py
├── core.py
├── subactions.py
└── test.py

##### core内容详解
1. 定义一个action迭代器
2. 定义send方法
3. 定义run方法
4. 描述一个装饰器方法

```
class Action(object):
    def __init__(self, func, args=[], kwargs={}):
        self.name = func.func_name
        self.func = func
        self.args = args
        self.kwargs = kwargs
        self.is_running = False
        self.is_done = False
        self.iterator = None
        self.result = None

    def __iter__(self):
        return self

    def next(self, data=None):
        if self.is_done:
            raise StopIteration
        if not self.is_running:
            result = self.func(*self.args, **self.kwargs)
            if hasattr(result, '__iter__'):
                self.is_running = True
                self.iterator = result
            else:
                self.is_done = True
                self.result = result
                return result
        if self.is_running:
            try:
                if data:
                    result = self.iterator.send(data)
                else:
                    result = next(self.iterator)
            except StopIteration, e:
                self.is_done = True
                self.is_running = False
                raise e
            return result

    def send(self, data):
        return self.next(data)

    def run(self):
        try:
            result = self.next()
            while True:
                if isinstance(result, Action):
                    action_result = result.run()
                    result = self.send(action_result)
                else:
                    self.send(result)
        except StopIteration:
            pass
        return self.result


def action(func):
    def wrapper(*args, **kwargs):
        return Action(func, args, kwargs)
    return wrapper
```

##### actions 详解
1. 编写调用原子化的业务逻辑代码
2. 使用yield关键字创建生成器函数

```
from core import action
from subactions import get_stock, modify_stock, create_order, record_error

@action
def place_order(user, products):
    for product in products:
        stock = yield get_stock(product)
        if stock > 0:
            yield modify_stock(product, -1)
            yield create_order(user, product)
        else:
            yield record_error(user, product)
```


##### subactions 详解
1. 编写正常的原子化业务逻辑函数
2. 添加action装饰器

```
from core import action

stock_store = {
    "0001": 10,
    "0002": 10,
}

order_store = []

@action
def get_stock(product):
    print "get stock: %s" % product
    return stock_store[product]

@action
def modify_stock(product, amount):
    print "modify stock: %s, %s" % (product, amount)
    stock_store[product] = stock_store[product] + amount

@action
def create_order(user, product):
    print "create order: %s, %s" % (user, product)
    order_store.append((user, product))

@action
def record_error(user, product):
    print "record_error: %s, %s" % (user, product)
```

##### test 详解
1. 编写测试代码
2. 使用定义的的迭代器方法进行调试

```
from actions import place_order
from subactions import modify_stock


def test_place_order():
    action = place_order("user1", ["0001"])
    n = action.next()  # 获取迭代器对象
    print n.name == 'get_stock'
    n = action.send(10)
    print n.name == 'modify_stock'
    action = place_order("user1", ["0001"])
    n = action.next()
    n = action.send(0)
    print n.name == 'record_error'


if __name__ == '__main__':
    place_order("user1", ["0001"]).run()  # 业务调用
    test_place_order()  # 测试调用

```

