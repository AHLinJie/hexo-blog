---
layout: post
title: ab测试
date: 2015-8-21
category: 测试
tag: testing
---
**摘要:**
web 的ab测试可以提现一个站点的性能情况,在必要的时候提升网站的性能不仅仅可以
提升用户体验,更是可以提高站点运营的收益.google上面可以找到很多的ab testing的
工具.这里介绍下一个常用的工具



#### 安装
sudo apt install apache2-utils

#### 执行命令
ab -n 100 -c 10 http://www.baidu.com/  # 100请求数 并发10


```
This is ApacheBench, Version 2.3 <$Revision: 1706008 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking www.baidu.com (be patient).....done


Server Software:        BWS/1.1
Server Hostname:        www.baidu.com
Server Port:            80

Document Path:          /
Document Length:        102165 bytes

Concurrency Level:      20
Time taken for tests:   1.067 seconds
Complete requests:      100
Failed requests:        99
   (Connect: 0, Receive: 0, Length: 99, Exceptions: 0)
    Total transferred:      10115466 bytes
    HTML transferred:       10021980 bytes
    Requests per second:    93.70 [#/sec] (mean)
    Time per request:       213.446 [ms] (mean)
    Time per request:       10.672 [ms] (mean, across all concurrent requests)
    Transfer rate:          9256.11 [Kbytes/sec] received

    Connection Times (ms)
                  min  mean[+/-sd] median   max
                  Connect:        6   25  12.5     25      50
                  Processing:     6  147  87.0    124     502
                  Waiting:        5   33  12.7     32      72
                  Total:         14  172  83.5    151     509

                  Percentage of the requests served within a certain time (ms)
      50%    151
      66%    167
      75%    192
      80%    199
      90%    208
      95%    426
      98%    454
      99%    509
      100%    509 (longest request)
```
