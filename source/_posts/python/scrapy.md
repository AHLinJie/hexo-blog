---
layout: post
title: 定时爬虫任务实验
date: 2016-10-05
category: python
tag: scrapy
---
**摘要:**
最近有个自己写代码需求,就是需要定时执行爬虫任务去爬去网页的信息,写在这里做备忘.
使用的是scrapy和django,其实里面的item还是很好用的有空可以深入研究下.django配合
python-scrapyd-api触发scrapyd爬虫服务器执行爬虫代码,django-celery-beat来做定时
还是很方便的.


#### 打包爬虫代码提交到爬虫服务器

1. 安装setuptools
wget https://bootstrap.pypa.io/ez_setup.py -O - | python
[参考网址](https://pypi.python.org/pypi/setuptools)

2. egg打包方法：
[参考网址](http://setuptools.readthedocs.io/en/latest/setuptools.html)

3. 添加setup.py文件到项目目录中
编辑该文件：
```
from setuptools import setup, find_packages

setup(
            name="govbuyscrapy",
                version="0.1",
                    packages=find_packages(),
                        entry_points={'scrapy': ['settings = govbuyscrapy.settings']},
                            # to make sure scrapyd can import you "scrapy" package and set you project setting file
     )
```

4. 打包项目生成egg文件
  4.1 python setup.py bdist_egg 将会在dist目录生成egg文件
  4.2 python setup.py bdist 是生成tar.ge文件但是不生成egg文件

5. 像scrapyd服务器提交爬虫项目
curl http://localhost:6800/addversion.json -F project=govbuyscrapy -F version=0.1 -F egg=@govbuyscrapy-0.1-py2.7.egg
在终端看到下面的表示提交成功
{"status": "ok", "project": "govbuyscrapy", "version": "0.1", "spiders": 3, "node_name": "jeep"}

6. 测试执行爬虫
命令scrapyd服务器执行指定爬虫 
curl http://localhost:6800/schedule.json -d project=govbuyscrapy -d spider=govbuy_wan_huoshan
执行结果:
{"status": "ok", "jobid": "0c838fd4b9f111e6abcc14dda97ae760", "node_name": "jeep"}
可以在浏览器里面(爬虫服务器)查看执行结果和log记录
http://localhost:6800/jobs


#### 在django项目中使用python-scrapyd-api来操作爬虫的活动
1. 安装python-scrapyd-api
pip install -i http://pypi.douban.com/simple/ python-scrapyd-api  --trusted-host pypi.douban.com
[参考文档地址](https://pypi.python.org/pypi/python-scrapyd-api#downloads)
这里有一个奇怪的事情： 我以为每次改动爬虫项目代码的之后倒要去打包生成新的egg并上
传到scrapyd服务器，但是没有这样做新写的代码也生效了

2. 安装rabbitmq（消息队列）
http://www.rabbitmq.com/download.html  下载deb点击安装即可(ubuntu系统)

3. 了解下celery（任务队列）
任务队列是一种在线程或机器间分发任务的机制。
消息队列的输入是工作的一个单元，称为任务，独立的职程（Worker）进程持续监视队列中
是否有需要处理的新任务。Celery 用消息通信，通常使用中间人（Broker）在客户端和职程
间斡旋。这个过程从客户端向队列添加消息开始，之后中间人把消息派送给职程。
Celery 系统可包含多个职程和中间人，以此获得高可用性和横向扩展能力。
  3.1 启动celery :
    `celery -A gadmin worker -B -l info` # 注意部署的时候不要使用 -l info  -B表示 --beat
    **注意:** celery 4.0 开始支持定时任务了 不需要djcelery

  3.2 在项目目录添加tasks包（含__init__.py）
  3.3 添加配置文件celerycfg.py
    ```
    CELERY_IMPORTS = ("tasks",)
    CELERY_TASK_RESULT_EXPIRES = 300
    ```
  3.4 import到django项目setting.py中
    可以在shell中执行测试的task.delay()

#### 定时任务
[参考网址](https://github.com/celery/django-celery-beat)
1. 安装django-celery-beat
`pip install -U django-celery-beat`
2. 添加install_app到django INSTALL_APPS中
3. 创建数据库表
`python manage.py migrate`

