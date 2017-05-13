---
layout: post
title: numpy笔记
date: 2016-08-31
category: python
tag: numpy
---

**摘要:**
一直想抽空学习numpy,想用numpy做一些数据分析,比如说哪个妹子跟好看呀!
numpy使用c和c++开发的python数据处理计算分析库,在数据挖掘和分析领域
很受欢迎,也有一些其他的python库是基于numpy开发的,比如说pandas.有很多
没来得及记录下来,下次学习的时候顺便补上学习内容.

#### VWAP: 成交量加权平均价格

某个价格成交量越高，该价格所占的权重就越大；成交量为权重计算出来的加权平均值。

#### TWAP: 时间加权平均价格

最近的数值（价格）重要性要大些，具体问题具体分析。

#### 最大值和最小值

```
import numpy as np

c, v = np.loadtext('data.csv', delimiter=',', usecols=(4, 5), unpack=True)

# 加权平均值
vwap = np.average(c, weights=v)  

# 算数平均值
mean = np.mean(c) 

# 时间加权平均值

t = np.arange(len(c))  # 加权方式
twap = np.average(c, weights=t)

# 最大值和最小值
h, l = np.loadtxt('yahoofinacedata/table.csv', delimiter=',', usecols=(3, 4), unpack=True)


```

- [网址](http://www.numpy.org/)

