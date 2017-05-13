---
layout: post
title: 阿里云服务操作
date: 2016-11-20
tag: 阿里云
category: 运维
---

**摘要:** 
记录阿里云停用线上服务器的方法,其中包含了阿里云的后台操作和主机操作,相关的docker
部署操作,内容比较繁杂,需要小心处理,谨慎处理线上问题保证在没有流量的情况下做操作,
以免线上用户体验问题.有些地方应该有更好的解决方案,避免手动处理这些问题,可以采用
运维自动化的方案.



### 1. 按量服务器　停用方法
#### 1.1 关闭主机上面的相应服务 **确保没有流量了, 从Nginx关闭流量**
1. ssh root@ip.address
2. docker ps
3. docker stop celery1 celery2 celery3 celery4  # 关闭celery （关闭worker）
4. docker ps
5. 在阿里云后台停止　对应的主机->管理－>（获取验证码）停止　
6. 重新初始化磁盘将进入本实例磁盘中，请根据需要单选或者多选执行重新初始化磁盘操作（需要手机验证码）
***如果是更换主机（重新购买新主机则不需要清理磁盘）***

### 2. 新机软件及更新
1. 启动主机
5. 阿里云后台　重置　该实例的密码（ssh登陆密码）重启主机
6. ssh root@ip.address

#### 2.1 更新ubuntu的更新源为阿里源
1. wget http://7xogkj.com1.z0.glb.clouddn.com/file/sources.list.trusty   # 获取源(上传七牛的源文件链接)
2. cp /etc/apt/sources.list /etc/apt/sources.list.bk; cp ./sources.list.trusty /etc/apt/sources.list  # 备份原有源 + 更换源

#### 2.2 更新系统软件
1. sudo apt-get update
2. sudo apt-get install curl -y

#### 2.3 安装阿里云docker
1. curl -sSL http://acs-public-mirror.oss-cn-hangzhou.aliyuncs.com/docker-engine/internet | sh -
echo "DOCKER_OPTS=\"--registry-mirror=https://n5fb0zgg.mirror.aliyuncs.com\"" | sudo tee -a /etc/default/docker
2. service docker restart


#### 2.4 docker注册登陆
1. docker login --username=<镜像登陆账号> registry.aliyuncs.com -p <镜像登陆密码>
2. docker run --name static -v /data busybox /bin/sh 

### 3. 项目部署
#### 3.1 拷贝项目需要的目录及文件
- 老版本已经弃用
```
1. docker inspect static　# 查看静态目录路径 "Name": "/static" 中"Mounts" "Source": "/var/lib/docker/volumes/123456/_data"
2. cd /var/lib/docker/volumes/123456/_data  # 进入该静态目录
3. mkdir fonts   # 创建字体目录
4. mkdir -p log/django  # 创建log目录
```

- 新方法
1. 登陆到root@sale9.example.com 清理 /var/data/log/django 中的log文件信息为空**前提是这里的log信息已经在其他地方保存了(例如kibana)**
2. scp -r root@sale9.example.com:/var/data/ /var/  # 拷贝其他生产服务器的目录到新主机(**如果其他主机的log文件过大又不能清空则要手动touch文件**)

#### 3.2 添加redis　ip白名单

#### 3.3 外网测试下　0.0.0.0:9000
1. 修改本地主机地址映射 在　/tec/hosts  文件中添加行
<新主机的ip地址>  admin.example.com　　# 其中＂.example.com＂　项目配置 ALLOWED_HOSTS 中选择
2. 在阿里云服务器启动docker **测试环境**
docker run --name=gunicorn --restart=always -e INSTANCE=mall -e TARGET=production -e MYSQL_AUTH=<mysql-auth密码> \
-e REDIS_AUTH=<rds-auth密码>  -v /var/data:/data -d -p 0.0.0.0:9000:9000 \
-e BLUEWARE_CONFIG_FILE=blueware.ini registry.aliyuncs.com/xiaolu-img/xiaolusys:latest blueware-admin run-program \
gunicorn -k gevent -c gunicorn_config.py -w 4 shopmanager.wsgi
**gunicorn_config.py 以及静态路径的挂在 按照项目中drone.yml中填写**
3. 本地浏览器访问admin.example.com:9000/rest/v1/(可以访问则表示成功)
4. docker stop <container-id> ; docker rm <container-id>  # 停用测试容器并删除
5. 启动项目(实际使用) **生产环境**
docker run --name=gunicorn --restart=always -e INSTANCE=mall -e TARGET=production -e MYSQL_AUTH=<mysql-auth密码> \
-e REDIS_AUTH=<rds-auth密码>  -v /var/data:/data -d -p `ifconfig eth0 | awk '/inet addr/{print substr($2,6)}'`:9000:9000 \
-e BLUEWARE_CONFIG_FILE=blueware.ini registry.aliyuncs.com/xiaolu-img/xiaolusys:latest blueware-admin run-program \
gunicorn -k gevent -c gunicorn_config.py -w 4 shopmanager.wsgi


#### 3.4 持续集成key拷贝
***将自己本地主机的sshkey拷贝到远程(下次登陆不需要密码输入了)***
1. ssh-copy-id root@sale7.example.com  # 目的是新主机上面生成 .ssh/authorized_keys 文件
2. 然后登陆线上其他主机将authorized_keys中的持续集成的key都拷贝到新主机的authorized_keys里面  # 保证持续集成

#### 3.5 添加地址解析
在阿里云后台 云解析DNS　中找到对应的解析域名　添加解析记录　保存　**注意同名同ip的情况**

#### 3.6 修改项目drone.yml 
1. 持续集成配置文件 添加部署的域名

### 3.7 NGNIX CONFIG
**一定要确保上述配置被核实成功　没有错误才可以在nginx上分配用户流量到该新主机**
1. 修改配置
2. push 到staging ｓｕｃｃｅｓｓ之后　访问admin.example.com/admin  测试配置是否影响访问
3. 第二步访问正常则push 到master
**gunicorn　负载超过50%的时候则不能再分配流量**

