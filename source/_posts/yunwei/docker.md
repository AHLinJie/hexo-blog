---
layout: post
title: Docker 基础
date: 2016-03-06
category: 运维
tag: docker 
---

**摘要:**
我只想说,docker如火如荼,大家都得学习学习,建议看官方文档的好,官方文档很详细.


#### Docker Install & Base Commands

$> curl -fsSL https://get.docker.com/ | sh

After installing Docker:

A user needs to be added to the docker group.

$> sudo usermod -aG docker

The docker daemon needs to be started

$> sudo service docker start

You can set the daemon to start at boot

$> sudo chkconfig docker on

You can verify the docker service is running

$> service docker status

And one last final check

$> docker run hello-world


1. django collect static files by docker 
   `docker run -ti -v "$PWD":/usr/src/app -w /usr/src/app/shopmanager test-ubuntu-python python manage.py collectstatic`
   `docker run -ti -v "当前目录"为app -w 项目空间 docker镜像　要执行命令`

2. `docker ps -a `:  show docker process all

3. `docker logs <container-id>` : show the assign container logs

[官方文档](https://docs.docker.com/)
[中文文档](http://www.docker.org.cn/book/docker.html)

4. dockerremove the dangling images  
4.1 docker images --quiet --filter=dangling=true  # 列出dangling镜像   
4.2 docker images --quiet --filter=dangling=true|xargs --no-run-if-empty docker rmi -f  ＃　删除dangling镜像, 释放磁盘空间
5. 删除容器(过滤)  
  5.1 docker ps --filter "status=exited" | grep 'weeks ago' | awk '{print $1}' | xargs --no-run-if-empty docker rm
6. commit container as images  
  6.1 docker commit <container-id> <repo-name>:<tag>
7. docker run -it -v ~/Downloads:/home/Downloads ubuntu/python3:python3.5 bash
---

#### DJANGO DOCKER

1. docker pull django
2. set volume and poort by run docker with django docker image
2.1 docker run -p 9002:9001 -ti -v /home/jishu_linjie/studyspace/django-stu/myproject:/home:ro django /bin/bash
cd home/myproject
3. runserver in docker
python manage.py runserver 0.0.0.0:9001
4. verify 
open in host broswer : http://192.168.1.31:9002  

5. orther docker cmd : 2.2 docker run -d -p 9002:9001 -ti -v /home/jishu_linjie/studyspace/django-stu/myproject:/home:ro django /home/myproject/manage.py runserver 0.0.0.0:9001


---
#### Docker File

```
FROM ubuntu:16.04
RUN apt-get update
RUN apt-get install -y gcc  # ./configure 才可以执行
RUN apt-get install -y build-essential # make 才可以执行
RUN apt-get install -y libssl-dev zlib1g-dev libbz2-dev libsqlite3-dev # 才可以编译安装python3.5
```
