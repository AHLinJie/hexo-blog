---
layout: post
title: linux 常用命令
date: 2014-08-10
category: linux
tag: linux
---
**摘要:**
在使用ubuntu的过程中有一些需要记住的命令,这里记录下方便以后查阅.


#### Ubuntu Usually Commands

- clean the dpkg commands:
  - `apt-get autoclean`
  - `apt-get -f install`
  - `dpkg --configure -a`
  - `apt-get -f install`
  - `apt-get -u dist-upgrade`
  - `or try "aptitude" instead of "apt-get"`

- ubuntu suspend command
  - `sudo bash -c "sleep 1s; pm-suspend"`


- clean the memory command
  - `echo 1 > /proc/sys/vm/drop_caches`


- ssh command
  - `ssh fang@192.168.1.8`
  - `scp filename username@192.168.1.31:/home/user/dir`
  - `ssh-keygen -t rsa -C " xxx@xx.com"` # ssh file 
  - `sshfs user@hostip:/dir/path ~/local/dir/path`  # 映射本地目录到远程主机

- tar commands
  - `tar cvf target.tar dirname/` // tar file

- rm commands
  - `find . -name "*.pyc" | xargs rm -f` // romove some category file

- show linux core info
  - `cat /proc/version`
  - `username -r` 
  - `username -a`

- disk 

  - df -h 
  - cd /var/lib
  - du -hs  # 查看当前目录各文件大

- awk
  `lsb_release -a|awk '{print $1$2}'|awk 'NR>=2'` // 取列输出 nr 取行输出

#### ENOSPC 解决方法

```
[2016-10-25 09:48:57,785 pyinotify ERROR] add_watch: cannot watch /usr/local/lib/python2.7/dist-packages/django/contrib/messages/storage/cookie.py WD=-1, Errno=No space left on device (ENOSPC)
```
1. $cat /proc/sys/fs/inotify/max_user_watches
2. $sudo sysctl fs.inotify.max_user_watches=16384 (修改数量变大)
[参考链接](http://www.cnblogs.com/yasmi/p/5192694.html)

---

awk

xargs


---
#### 添加任务栏快捷方式(可执行文件)

1. 创建快捷键文件  
  1.1 /usr/local/share/applications/aplication-name.desktop  (/usr/share/applications/)
2. 修改文件

```
[Desktop Entry]
Type=Application
Terminal=true
Name=aplication-name
Icon=/path/to/icon/icon.svg
Exec=/path/to/file/mount-unmount.sh
```
3. 拖动文件到任务栏



---
#### 一个烂脚本
```
#!/bin/sh
CURRENT_USER=`who |awk '{print $1}'i`
echo "Hello curent user name is $CURRENT_USER, now we will get some info from this computer with the user !"

echo "Excute cmd:w $CURRENT_USER \n"
w $CURRENT_USER

echo "\n"

echo "Current Kernel Is: \n"
echo `uname -r`

echo "\n"

echo "Current System Is: \n"
echo "`lsb_release -a` \c"

echo "\n"

echo "Active Service: \n"

echo "`service --status-all | grep +` \c"

echo "\n"

echo "Open Ports: \n"
echo "`lsof -i` \c"

echo "\n"
echo "NetWork Status: \n"  # [Referance](https://www.cyberciti.biz/faq/check-network-connection-linux/)
echo "`netstat -s` \c"

echo "\n"
echo "Disk Status : \n"
# [Referance](http://www.binarytides.com/linux-command-check-disk-partitions/)
echo "`df -h` \c"


echo "\n"
echo "Memery Info: \n"
# [Referance](https://www.linux.com/blog/5-commands-check-memory-usage-linux)
echo "`free -m` \c"

echo "\n"
echo "Cpu Info: "
echo "`lscpu` \c"


echo "\n"
echo "Cpu Status: \n"
# [Referance](https://www.cyberciti.biz/tips/how-do-i-find-out-linux-cpu-utilization.html)
echo "`top|head -8` \c"

```
