---
layout: post
title: curl 的使用
category: tools
tag: curl
date: 2016-8-10
---
**摘要:**
curl可以用来发送一些客户端请求,http,ftp等等，可以指定http的版本等等。
这里做一些记录。不断更新中。


##### GET
curl http://localhost:8888/test/?username=linjie&password=123455


##### POST
curl -X POST -F 'username=linjie' -F 'password=123456' http://localhost:8888/test/

##### HEAD
curl -I http://localhost:8888/test/

#### cookie 以及　post json 文件 
curl -b "@hrcommon/login_d1_main_cookie.txt"  -H "Content-Type: application/json" -X PUT -d "@hr35/impression.json" http://localhost:8000/api/account/template/company_impression

