---
layout: post
title: 守护进程
date: 2015-12-28
category: 运维
tag: supervisor
---

**摘要:**
生产服务器上面运行这很多重要的程序,有些程序不是很稳定或者是系统原因会停止运行
可以使用守护进程来做守护处理,保证重要的程序一直处于执行状态.

#### Supervisor Base Option


##### 安装 

`sudo easy_install supervisor` or `sudo pip install supervisor`

##### 配置

1. gen config file (put the config file in `etc` director):

    `echo_supervisord_conf > /etc/supervisord.conf`
    
    **如果配置错误修改该配置文件后使用更新命令:** `supervisorctl update`

2. add the guard process(edit the config file,for example)
    
    ```
    [program:testpy] ;; testpy是需要守护的进程
    command=python /home/jishu_linjie/testpy.py  ;; 要执行的命令
    autorestart=true　;; 开启自动重启
    ```

##### 测试下
    
```
root@acer-lin:/home/jishu_linjie# emacs /etc/supervisord.conf 
root@acer-lin:/home/jishu_linjie# supervisorctl update
testpy: stopped
testpy: updated process group
root@acer-lin:/home/jishu_linjie# supervisorctl 
testpy                           STARTING  
supervisor> restart testpy
testpy: stopped
testpy: started
supervisor> stop testpy
testpy: stopped
supervisor> 
```

```
supervisorctl reload
supervisorctl stop all
```

##### 验证
1. guarded process(testpy.py):
        
```
#!/usr/bin/python
import datetime
import time
import os

while 1:
    time.sleep(2)
    f = open('supervisor.log','a')
    t = time.time()
    f.write(str(t))
    f.write("\n")
    f.close()
```
```
python testpy.py
CTRL + C　to kill the process, 　log still increce, except stop the testpy in `supervisordctl`
``` 
##### 注意
The command `supervisorctl stop all` will kill the guarded process all and the `reload` and `update` command can not awaken the killed process.





