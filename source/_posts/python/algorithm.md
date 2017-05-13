---
layout: post
title: 一些算法
date: 2016-04-20
category: 算法
tag: 算法
---

**摘要:**
谈到算法有两种感觉,第一个感觉是高大上,第二个感觉有难度.高大上是因为那些研究算法的
人都是一些大牛,并且在数学上面有很深的造诣.很钦佩和尊敬那些在算法事业上的牛人.说算
法有难度,是针对个人来说的,我对算法很感兴趣,但是总是浅尝辄止,有的复杂算法就了解下
一带而过了,希望能在这里记录写学习算法的点滴不管是容易的算法还是难的算法还是亲身经历
下得好!


---

#### Vector Correlation Algrithm
图片相似度中使用到的算法.

- Correlation Section[-1, 1]
** 0:lower 1: higher . And the negative is negative correlations**

```
#coding=utf-8  
import sys
# input 2 vector array  
# output pearson correlation score  

def PearsonCorrelationSimilarity(vec1, vec2):  
    value = range(len(vec1))  
  
    sum_vec1 = sum([ vec1[i] for i in value])  
    sum_vec2 = sum([ vec2[i] for i in value])  
  
    square_sum_vec1 = sum([ pow(vec1[i],2) for i in  value])  
    square_sum_vec2 = sum([ pow(vec2[i],2) for i in  value])  
  
    product = sum([ vec1[i]*vec2[i] for i in value])  
  
    numerator = product - (sum_vec1 * sum_vec2 / len(vec1))   
    dominator = ((square_sum_vec1 - pow(sum_vec1, 2) / len(vec1)) * (square_sum_vec2 - pow(sum_vec2, 2) / len(vec2))) ** 0.5  
  
    if dominator == 0:  
        return 0  
    result = numerator / (dominator * 1.0)  
  
    return result  
  
vec1 = [5.0, 3.0, 2.5]
vec2 = [2.0, 2.5, 5.0]  
vec3 = [5.0, 3.0, 0]  
print (PearsonCorrelationSimilarity(vec1, vec2))
print (PearsonCorrelationSimilarity(vec1, vec3))
```

[Theory](http://www.ruanyifeng.com/blog/2013/03/similar_image_search_part_ii.html)

[Refrence Link](http://blog.csdn.net/afeionepiece/article/details/47624465)

----

#### Otsu Algorithm(大津算法)
图片处理过程中使用到的算法.

- Introduce
    `w1 = n1 / n` :n1表示小于灰度阀值的像素个数　n 表示像素总个数　w1　表示前者所占比例
    `w2 = n2 / n` :n2表示大于灰度阀值的像素个数
    `类间差异 = w1w2(μ1-μ2)^2`:穷举最低像素灰度值到最高像素灰度值之间的筏值，计算出类间差异最小

[Theory](http://www.labbookpages.co.uk/software/imgProc/otsuThreshold.html)

[Refrence Link](http://www.ruanyifeng.com/blog/2013/03/similar_image_search_part_ii.html)


---
#### python实现stack
```
#coding=utf-8

class Stack(object):
    def __init__(self):
        self.items = []

    def isEmpty(self):
        return len(self.items) == 0

    def push(self, item):
        self.items.append(item)

    def pop(self):
       return self.items.pop()

    def peek(self):
       if not self.isEmpty():
           return self.items[-1]

    def size(self):
        return len(self.items)

```

#### 逆波兰表达式(后缀表达式)
[java实现](http://www.programcreek.com/2012/12/leetcode-evaluate-reverse-polish-notation/)
以下是Python实现.

```
["2", "1", "+", "3", "*"] -> ((2 + 1) * 3) -> 9
["4", "13", "5", "/", "+"] -> (4 + (13 / 5)) -> 6
```
1. 简单的实现方法
```
from stack import Stack

a = Stack()
s = ["2", "1", "+", "3", "*"]

for i in s:
   if i.isdigit():
       a.push(i)
   else:
       x = a.pop()
       y = a.pop()
       if i == '+':
           a.push(int(x) + int(y))
       if i == '-':
           a.push(int(y) - int(x))
       if i == '*':
           a.push(int(x) * int(y))
       if i == '/':
           a.push(int(y) / int(x))
result = a.pop()
```
2. 公认的实现方法
```
from stack import Stack

stack = Stack()
s = ["2", "1", "+", "3", "*"]
operators = '+-*/'

for i in s:
   if i not in operators:
       stack.push(i)
   else:
       x = int(stack.pop())
       y = int(stack.pop())
       index = operators.index(i)
       if index == 0:
           stack.push(x + y)
       if index == 1:
           stack.push(y - x)
       if index == 2:
           stack.push(x * y)
       if index == 3:
           stack.push(y / x)
```

---

#### 最长回文算法
注意这里的算法复杂度和空间复杂度.以下是Python实现.
[java实现](http://www.programcreek.com/2013/12/leetcode-solution-of-longest-palindromic-substring-java/)
```
#coding=utf-8
import sys


def longest_palindrome(s):
    """
    Time O(n^2), Space O(n^2)
    """
    if not s:
        return

    slen = len(s)
    max_len = 1

    dp = []

    # init the zero array, Space 
    for x in xrange(slen):
        d = []
        for y in xrange(slen):
            d.append(0)
        dp.append(d)

    longest = ''

    # condition judgement, Time
    for l in range(slen):
        for i in range(slen - l):
            j = i + l
            if s[i] == s[j] and (j - i <= 2 or dp[i + 1][j - 1]):
                dp[i][j] = 1

                if j - i + 1 > max_len:
                    longest = s[i: j + 1]
    return longest

def simple_longest_plaindrome(s):
    """
    Time O(n^2), Space O(1)
    """
    slen = len(s)

    if not s:
        return
    if slen == 1:
        return s

    longest = s[0:1]

    # 从中间向两端查找直到符合回文
    def helper(s, begin, end):
        while begin >=0 and end <= len(s) -1 and s[begin] == s[end]:
            begin -= 1
            end += 1
        return s[begin + 1: end]

    for i in xrange(slen):
        tmp = helper(s, i, i)
        if len(tmp) > len(longest):
            longest = tmp

        tmp = helper(s, i, i + 1)
        if len(tmp) > len(longest):
            longest = tmp;

    return longest

if __name__ == '__main__':
    s = sys.argv[1]
    l = longest_palindrome(s)
    print l
    l2 = simple_longest_plaindrome(s)
    print l2
```



---

#### 单词分割

给一个字符串s 和一个字典d(python用list表示),如果s可以从d中挑选一个或多个拼接则表示这个s可以被d分割.
s = "leetcode",
dict = ["leet", "code"].

以下是Python实现.
[Java实现](http://www.programcreek.com/2012/12/leetcode-solution-word-break/)
```
#coding=utf-8
import sys


class WordBreak(object):
    def __init__(self, s, d):
        self.string = s
        self.dictionary = d

    def isWordBreak(self, simple=True):
        if simple:
            return self.simpleBreakHelper(self.string, self.dictionary)

        return self.wordBreakHelper(self.string, self.dictionary, 0)

    def wordBreakHelper(self, s, d, start):
        """将目标字符串的对应位置(有start和dictionary决定)查找与在字典内相等的
        字符串,如果相等则从下一个位置继续查看是否匹配
        """
        if start == len(s):
            return True

        for a in d:
            al = len(a)
            end = start + al
            if end > len(s):
                continue
            if s[start: end] == a:
                if self.wordBreakHelper(s, d, end):  # 递归
                    return True
        return False

    def simpleBreakHelper(self, s, d):
        """当字典很大的时候采用这个方法O(n^2) n 表示字符串的长度
        穷举设置标志位
        """
        pos = []
        slen = len(s)
        
        for x in xrange(slen + 1):
            pos.append(-1)
        pos[0] = 0

        for i in xrange(slen):
            if pos[i] != -1:
                for j in xrange(i + 1, slen + 1):
                    sub = s[i: j]
                    if sub in d:
                        pos[j] = i
        return pos[slen] != -1


if __name__ == '__main__':
    s = sys.argv[1]
    d = sys.argv[2].split(',')

    wb = WordBreak(s, d)
    print wb.isWordBreak()
```

