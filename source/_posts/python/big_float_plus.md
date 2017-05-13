---
layout: post
title: 长浮点数相加问题
category: python
date: 2017-02-06
tag: 算法
---

**摘要:**
今天面试,面试官王轶凡老师出的一道题目,就是给两个很长的浮点字符串;输出相加后的结果.
保证没有精度丢失.这里记录下备忘,以后再研究.

##### 自己写的代码如下

没有去做优化, 感觉空间蛮大.哈哈......

```
# coding=utf-8
import sys


def main(a, b):
    ia = a.index('.')
    ib = b.index('.')

    l_a = len(a)
    l_b = len(b)
    t_a = l_a - ia
    t_b = l_b - ib
    dot_index = max(ia, ib)

    a = list(a)
    b = list(b)

    a.remove('.')
    b.remove('.')
    # 向前补0
    if ia > ib:
        for i in xrange(ia - ib):
            b.insert(0, 0)
    else:
        for i in xrange(ib - ia):
            a.insert(0, 0)

    # 向后补0
    if t_a > t_b:
        for i in xrange(t_a - t_b):
            b.append(0)
    else:
        for i in xrange(t_b - t_a):
            a.append(0)

    a.reverse()
    b.reverse()

    res = list('0' * max(l_a, l_b))
    tens = 0  # 保存十位数

    for i, j in enumerate(a):
        v1 = int(j) + tens
        v2 = int(b[i])

        tens = (v1 + v2) / 10
        units = (v1 + v2) % 10
        res[i] = units

    res[i + 1] = tens
    res.reverse()

    # 去除无意义的0值
    if res[0] in (0, '0'):
        res = res[1::]

    if tens > 0:
        dot_index += 1

    # 移动小数点
    res.insert(dot_index, '.')
    res = [str(r) for r in res]
    return ''.join(res)


if __name__ == '__main__':
    arg1 = sys.argv[1]
    arg2 = sys.argv[2]
    res = main(arg1, arg2)
    # res = main('999.99', '9.9')  # 1009.89
    # res = main('9.9', '999.99')  # 1009.89
    # res = main('999.991', '9.9')  # 1009.891
    # res = main('9.9', '999.991')  # 1009.891
    print res
```
