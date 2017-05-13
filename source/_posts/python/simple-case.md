---
layout: post
title: python代码片段
date: 2015-9-28
category: python
tag: python
---

**摘要:**
Python代码片段摘抄,一些基础内容和一些常用的脚本,方便以后使用查阅.


#### Python Stamp 
- stamp to datetime

```
import datetime
import time
timeStamp = 1381419600
In [130]: date_time
Out[130]: datetime.datetime(2013, 10, 10, 15, 40)
```

- stamp output str

```
timeStamp = 1381419600
timeArray = time.localtime(timeStamp)
time_output = time.strftime("%Y-%m-%d %H:%M:%S", timeArray)
time_output == "2013-10-10 23:40:00"
```

- stamp save to database

```
set_time = datetime.datetime.strptime(time_str, '%Y-%m-%d %H:%M:%S')
```

- date converse to datetime
```
datetime.datetime.combine(instance.sale_time, datetime.time(0, 0))
```

---
#### Python Basestring


```
          object
             |
         basestring
            / \
           /   \
         str  unicode
         
>>> string1 = "I am a plain string"
>>> string2 = u"I am a unicode string"
>>> isinstance(string1, str)
True
>>> isinstance(string2, str)
False
>>> isinstance(string1, unicode)
False
>>> isinstance(string2, unicode)
True
>>> isinstance(string1, basestring)
True
>>> isinstance(string2, basestring)
True
```

---

#### Python List Dict 

```
a=range(10)
b={}
b=b.fromkeys(a)
c=list(b.keys())
```

---

#### Python Class Attribute

```
class Student(object):
    @property
    def score(self):
        return self._score

    @score.setter
    def score(self, value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        if value < 0 or value > 100:
            raise ValueError('score must between 0 ~ 100!')
        self._score = value
```

- Python `@property` make a self function to a attribute to call
- `@property` gen a decrator `@score.setter`, the score is decratored by '@property', setter is '@property' to control the value for that attribute.

- before st.score assignment

```
In [54]: st._
st.__class__         st.__format__        st.__module__        st.__repr__          st.__subclasshook__
st.__delattr__       st.__getattribute__  st.__new__           st.__setattr__       st.__weakref__
st.__dict__          st.__hash__          st.__reduce__        st.__sizeof__        
st.__doc__           st.__init__          st.__reduce_ex__     st.__str__           

In [54]: st.score = 60
```

- after st.score assignment gen a _score attribute

```
In [55]: st._
st.__class__         st.__format__        st.__module__        st.__repr__          st.__subclasshook__
st.__delattr__       st.__getattribute__  st.__new__           st.__setattr__       st.__weakref__
st.__dict__          st.__hash__          st.__reduce__        st.__sizeof__        st._score
st.__doc__           st.__init__          st.__reduce_ex__     st.__str__           
```

- assignment test

```
In [55]: st._score=101

In [56]: st.score = 101
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
<ipython-input-56-c8cebab0a5ba> in <module>()
----> 1 st.score = 600

<ipython-input-50-363f41b24e8b> in score(self, value)
      8             raise ValueError("not int type")
      9         if value <0 or value>100:
---> 10             raise ValueError("out range 0-100")
     11         self._score = value
     12 

ValueError: out range 0-100
```

---

##### Dict List Summation

- [from](http://stackoverflow.com/questions/3490738/how-to-sum-dict-elements) 

```
>>> dict1 = [{'a':2, 'b':3},{'a':3, 'b':4}]
>>> from operator import itemgetter
>>> {k:sum(map(itemgetter(k), dict1)) for k in dict1[0]}        # Python2.7+
{'a': 5, 'b': 7}
>>> dict((k,sum(map(itemgetter(k), dict1))) for k in dict1[0])  # Python2.6
{'a': 5, 'b': 7}
```

---

#### Sort 

1. Dict List Sort
```
a = sorted(dict_list, key=lambda k: k[sort_field], reverse=True)
```
2. Dict Sort
```
import operator
x = {1: 2, 3: 4, 4: 3, 2: 1, 0: 0}
sorted_x = sorted(x.items(), key=operator.itemgetter(1))
```


---

#### Float Amount Decimal

```
  amount = int(decimal.Decimal(cashout_amount) * 100)
```

---

#### About Object Copy

```
from copy import deepcopy
t = ModeleName.objects.get(id=1)
t1 = deepcopy(t)
t1.id = None
t1.save()  # save the object as a django model instance
```

---

#### Python Map

- usually

```
def printlog(x):
    print "log", x
    return 
map(printlog, range(10))
```

- take a arg

```
def test(message):
    def _wrapper(x):
    	print "log", x , message
    	return 
    return _wrapper

map(test('hello world'), range(10))

```


---

#### pickle serializer

- write the memory data to hard disk
- get the same data in memory by call pickle again

```
import pickle

a = {
    "linjie": [170,120,31],
    "zhangwei": [176,140,34],
    "lintao": [170,120,30]
}# prepare data stracture

with file('pickle_test.pkl', 'wb') as f:
    pickle.dump(a, f) # serialize

f = file('pickle_test.pkl') # open a file
x = pickle.load(f)  # unserialize to load the data
f.close()   # close file
x == a
True
```

```
import pickle
         
data = {'foo': [1,2,3],
        'bar': ('Hello','world!'),
        'baz': True}
jar = open('data.pk1', 'wb')
pickle.dump(data, jar) # write pickle data to jar
jar.close()


import pickle
      
pk1_file = open('data.pk1','rb') # connect the pickle data
data = pickle.load(pk1_file) # loada data to var
print data
pk1_file.close()
```

- `attention１：` Dump could be many times，but store the last time data
- `attention２：` Throw a error when dump data is None
- `attention３：` Do close file when 


---

#### Python Mysql 

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


---

#### Python Connect SSH

```
#!/usr/bin/python
#-*- coding:utf-8 -*-
import sys, time, os

try:
    import pexpect
except ImportError:
    print "You must install pexpect module"
    sys.exit(1)

addr_map = {
'v3' :('root@192.168.1.162', 'sina@2009'),
'dev':('test016@192.168.1.136', 'test016'),
}

try:
    key = sys.argv[1]
    host = addr_map[key]
except:
    print "argv error, use it link jssh v3, v3 must defined in addr_map"
    sys.exit(1)
server = pexpect.spawn('/usr/bin/ssh %s' % host[0])
server.expect('.*ssword:')
server.sendline(host[1])
server.interact()

```

```
# coding=utf-8

import sys, time, os
try:
    import pexpect
except ImportError:
    print "You must install pexpect module"
    sys.exit(1)

git_map = {
    'dbnote': 'dbnote',
    'jsnote': 'jsnote',
    'ljnote': 'ljnote',
    'pynote': 'pynote',
    'other': 'other',
}

def main():
    try:
        key = sys.argv[1]
        git_dir = git_map[key]
    except:
        print "argv error"
        sys.exit(1)
    os.chdir('/home/jielin/workspace/gitbooks/%s' % git_dir)
    child = pexpect.spawn('git push origin master')
    child.expect('Username for .*:')
    child.sendline('you account for gitbook')
    child.expect('Password for .*:')
    child.sendline('you passwd for gitbook')
    child.expect(pexpect.EOF)
    print "push result: ", child.before

if __name__ == '__main__':
    try:
        main()
    except Exception, e:
        print str(e.message)
        sys.exit(0)

```

[Reference Link1](http://blog.csdn.net/lwnylslwnyls/article/details/8239791)

[Reference Link2](http://blog.csdn.net/lwnylslwnyls/article/details/8239791)

[Reference Link2](http://www.thinksaas.cn/topics/0/164/164786.html)



---

#### Dict List GroupBy

```
from itertools import groupby
from operator import itemgetter
x = [
    {'group': 'b', 'num': 4},
    {'group': 'a', 'num': 1},
    {'group': 'b', 'num': 3},
    {'group': 'b', 'num': 5},
    {'group': 'c', 'num': 6},
    {'group': 'a', 'num': 2},
]
x.sort(key=itemgetter('group'))  # must sort the list before groupby

groups = []
for g, items in groupby(x, key=itemgetter('group')):
    cate = {
        'name': g,
        'values': []
    }
    for i in items:
        cate['values'].append(i)
    groups.append(cate)
    
    
Out[5]: 
[{'name': 'a', 'values': [{'group': 'a', 'num': 1}, {'group': 'a', 'num': 2}]},
 {'name': 'b',
  'values': [{'group': 'b', 'num': 4},
   {'group': 'b', 'num': 3},
   {'group': 'b', 'num': 5}]},
 {'name': 'c', 'values': [{'group': 'c', 'num': 6}]}]

```

---

Python Email

```
# coding=utf-8
from __future__ import unicode_literals, absolute_import
import imaplib
import smtplib
import email
from email.header import decode_header
from email.mime.text import MIMEText


def fetch_mail():
    conn = imaplib.IMAP4_SSL("imap.exmail.qq.com", 993)
    conn.login("jie.lin@xiaolumeimei.com", "password")
    conn.list()
    conn.select("INBOX")
    type, data = conn.search(None, 'ALL')
    msgList = data[0].split()
    last = msgList[len(msgList) - 1]
    type, data = conn.fetch(last, '(RFC822)')
    msg = email.message_from_string(data[0][1])

    # print msg.keys()
    # ['Received', 'X-QQ-FEAT',
    # 'X-QQ-MAILINFO', 'X-QQ-mid',
    # 'X-QQ-ORGSender', 'DKIM-Signature', 'X-EnvId',
    # 'Received', 'Date', 'From', 'To', 'Message-ID',
    # 'Subject', 'MIME-Version', 'Content-Type']
    msg_from = decode_header(msg.get('From'))
    print msg_from, msg_from[0][0]
    # [('\xe9\x98\xbf\xe9\x87\x8c\xe4\xba\x91', 'utf-8'), ('<monitor@monitor.aliyun.com>', None)] 阿里云
    print msg.get_payload()
    # [<email.message.Message instance at 0x7f9b05fd4170>]
    msg = msg.get_payload()[0]
    content = msg.get_payload(decode=True)
    print content


def send_mail():
    # Open a plain text file for reading.  For this example, assume that
    # the text file contains only ASCII characters.
    # fp = open(textfile, 'rb')
    # Create a text/plain message
    # msg = MIMEText(fp.read())
    msg = MIMEText('hello world')
    # fp.close()

    # me == the sender's email address
    # you == the recipient's email address
    msg['Subject'] = 'The contents of %s' % 'www'
    msg['From'] = 'jie.lin@xiaolumeimei.com'
    msg['To'] = '1051631394@qq.com'

    # Send the message via our own SMTP server, but don't include the
    # envelope header.
    s = smtplib.SMTP_SSL('smtp.exmail.qq.com', 465)
    s.login("jie.lin@xiaolumeimei.com", "password")
    s.sendmail('jie.lin@xiaolumeimei.com', ['1051631394@qq.com'], msg.as_string())
    s.quit()

if __name__ == '__main__':
    fetch_mail()
    send_mail()

```


---

#### 脚本输入用户明密码

```
# coding=utf-8

import sys, time, os
try:
    import pexpect
except ImportError:
    print "You must install pexpect module"
    sys.exit(1)

git_map = {
    'dbnote': 'dbnote',
    'jsnote': 'jsnote',
    'ljnote': 'ljnote',
    'pynote': 'pynote',
    'other': 'other',
}

def main():
    try:
        key = sys.argv[1]
        git_dir = git_map[key]
    except:
        print "argv error"
        sys.exit(1)
    os.chdir('/home/jielin/workspace/gitbooks/%s' % git_dir)
    child = pexpect.spawn('git pull origin master')
    child.expect('Username for .*:')
    child.sendline('username')
    child.expect('Password for .*:')
    child.sendline('password here')
    child.expect(pexpect.EOF)
    print "pull result is :", child.before

    child = pexpect.spawn('git push origin master')
    child.expect('Username for .*:')
    child.sendline('username')
    child.expect('Password for .*:')
    child.sendline('password here')
    child.expect(pexpect.EOF)
    print "push result: ", child.before

if __name__ == '__main__':
    try:
        main()
    except Exception, e:
        print str(e.message)
        sys.exit(0)

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


