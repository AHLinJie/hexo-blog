---
layout: post
title: cProfile使用
category: tools
tag: performance
date: 2014-12-21
---

**摘要:**
python 性能调优工具介绍，有使用profile的，这里简单的介绍下cProfile
的使用方法．


##### 代码内部使用

```
def func():
    do sth

import cProfile
cProfile.run('func')
```

##### 命令行使用
1. `python -m cProfile -o pro -s ncalls tt.py argv_1`
-o 将标准输出到文件pro中
-s 表示将按照指定的列排序

##### pstats模块统计
对标准输出文件进行处理,统计。
```
import pstats
p = pstats.Stats('pro')  # 操作生成的标准输出文件
p.strip_dirs().sort_stats(-1).print_stats()  
# strip_dirs()  removed the extraneous path
# sort_stats()  sorted all the entries
# print_stats() print out all the statistics

p.sort_stats('name')
p.print_stats()

p.sort_stats('time').print_stats(10)   # 打印前面10最耗时的记录

```
##### 输出字段解释
- ncalls : 方法调用次数
- tottime : 调用总时间
- percall : 平均每次调用时间
- cumtime : 子函数调用总时间
- percall : 子函数平均调用时间
- filename:lineno(function) : 调用的方法名称
　
