---
layout: post
title: python 排序
date: 2015-06-01
tag: 算法
---

**摘要:**
排序在编程中经常用到,这里有些以前学过的排序算法，使用Python实现的。
封装在一个class中.以便查阅.


```
#coding=utf-8
import sys

class Xsort(object):
    def __init__(self, s):
        self.source = s 
        self.target = s

    def bubble_sort(self):
        # 冒泡排序O(n^2): 遍历序列长度,便利每一个元素与相邻元素比较交换
        l = len(self.target)

        while l > 0:
            for i in xrange(l - 1):
                # 交换位置
                if self.target[i] > self.target[i + 1]:
                    self.target[i] = self.target[i] + self.target[i + 1]
                    self.target[i + 1] = self.target[i] - self.target[i + 1]
                    self.target[i] = self.target[i] - self.target[i + 1]
            l -= 1
        return self.target

    def insert_sort(self):
        # 插入排序O(n^2)
        l = len(self.target)
        if l <= 1:
            return self.target

        for i in xrange(1, l):
            j = i -1
            if self.target[i] < self.target[j]:
                tmp = self.target[i]
                self.target[i] = self.target[j]

                # 向前找 插入到合适位置
                j = j - 1
                while j >= 0 and self.target[j] > tmp:
                    self.target[j + 1] = self.target[j]
                    j = j - 1

                self.target[j + 1] = tmp
        return self.target

    def selection_sort(self):
        # 选择排序
        l = len(self.target)

        for i in xrange(0, l):
            mi = i
            for j in xrange(i + 1, l):
                if self.target[j] < self.target[mi]:
                    mi = j 
                    self.target[i], self.target[mi] = self.target[mi], self.target[i]
        return self.target

    def heap_sort(self):
        # 堆排序O(nlog(n))
        l = len(self.target)

        def heaping(a, k, n):  # 堆处理
            N = n - 1
            while 2 * k <= N:
                j = 2 * k
                if j < N and a[j] < a[j + 1]:
                    j += 1
                if a[k] < a[j]:
                    a[k], a[j] = a[j], a[k]
                    k = j
                else:
                    break

        for i in range(l / 2, 0, -1):
            heaping(self.target, i, l)

        n = l - 1
        while n > 1:
            self.target[1], self.target[n] = self.target[n], self.target[1]
            heaping(self.target, 1, n)
            n -= 1
        return self.target[1:]
    
    def quick_sort(self, low=None, high=None):
        # 快速排序: 大小二分,递归调用
        i = 0 if low is None else low
        j = len(self.target) -1 if high is None else high
        
        if i >= j:
            return self.target
        key = self.target[i]

        while i < j:
            # 从后往前找, 直到一个元素大于等于 关键元素
            while i < j and self.target[j] >= key:
                j = j - 1
            self.target[i] = self.target[j]
            # 从前往后找, 直到一个元素小于等于 关键元素
            while i < j and self.target[i] <= key:    
                i = i + 1 
            self.target[j] = self.target[i]
        self.target[i] = key 

        self.quick_sort(low, i-1)
        self.quick_sort(j+1, high)

        return self.target


if __name__ == '__main__':
    source = [int(i) for i in sys.argv[1].split(',')]
    x = Xsort(source)
    # print x.bubble_sort()
    # print x.insert_sort()
    # print x.selection_sort()
    # print x.heap_sort()
    print x.quick_sort()

```

