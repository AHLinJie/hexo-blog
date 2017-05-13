---
layout: post
title: request基础笔记
category: http
tag: requests
date: 2016-12-27
---

**摘要:**
平时使用requests模块的基础内容,多用在爬虫或者http请求当中.

[Reques](http://docs.python-requests.org/zh_CN/latest/user/quickstart.html)
[状态码](https://zh.wikipedia.org/zh/HTTP状态码)

##### 基础使用
```
#coding=utf-8
import json
import requests
from requests import exceptions

URL = 'https://api.github.com'

def build_uri(endpoint):
    return '/'.join([URL, endpoint])

def better_print(json_str):
    return json.dumps(json.loads(json_str), indent=4)

def request_method():
    response = requests.get(build_uri('users/ahlinjie'))
    print better_print(response.text)

def params_request():
    response = requests.get(build_uri('users'), params={'since': 11})
    print better_print(response.text)
    print response.request.headers
    print response.url


def timeout_request():
    try:
        response = requests.get(build_uri('user/emails'), timeout=10)
        response.raise_for_status() # 状态码异常处理
    except exceptions.Timeout as e:
        print e.message
    except exceptions.HTTPError as e:
        print e.message
    else:
        print response.text
        print response.status_code

def hard_request():
    """
    自定义request
    """
    from requests import Request, Session

    s = Session()
    headers = {'User-Agent': 'fake1.3.4'}
    req = Request('GET', build_uri('users/ahlinjie'), headers=headers)
    prepped = req.prepare()
    print prepped.body
    print prepped.headers
    
    # 发送请求
    resp = s.send(prepped, timeout=5)
    print resp.status_code
    print resp.request.headers
    print resp.text
    
```

##### 下载图片的两种方法

1. 方法1
```
def download_image():
    url = 'http://img3.imgtn.bdimg.com/it/u=2228635891,3833788938&fm=21&gp=0.jpg'
    headers = {'User-Agent': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/55.0.2883.87 Safari/537.36'}
    # headers 不管用还是403
    headers_1 = {'User-Agent': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/55.0.2883.87 Safari/537.36','Referer': 'https://www.baidu.com'}
    # 经过测试 这里需要在headers 中添加Referer 并且是一个可以访问的

    response = requests.get(url, headers=headers_1, stream=True)
    print response.status_code # 403
    with open('demo.jpg', 'wb') as fd:
        for chunk in response.iter_content(128):
            fd.write(chunk)
```
2. 方法2
```
def download_img_improved():
    # 伪造headers信息
    headers = {'User-Agent': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/55.0.2883.87 Safari/537.36','Referer': 'https://www.baidu.com'}

    # 图片地址
    url = 'http://img3.imgtn.bdimg.com/it/u=2228635891,3833788938&fm=21&gp=0.jpg'

    from contextlib import closing
    
    # 及时关闭释放stream占用的资源
    with closing(requests.get(url, headers=headers, stream=True)) as response:
        # 写入文件
        with open('demo1.jpg', 'wb') as fd:
            # 每128个字节写入一次
            for chunk in response.iter_content(128):
                fd.write(chunk)
```

##### hook回调
```
def get_key_info(response, *args, **kwargs):
    # 回调执行
    print response.headers['Content-Type']

def event_hooks():
    requests.get('https://www.baidu.com', hooks=dict(response=get_key_info))

```

##### oauth认证
```
from requests.auth import AuthBase


class GithubOauth(AuthBase):
    def __init__(self, token):
        self.token = token

    def __call__(self, r):
        r.headers['Authorization'] = ' '.join(['token', self.token])
        return r


def oauth_advanced():
    github_token = ''  # 在自己的github setting 中生成一个
    auth = GithubOauth(github_token)
    response = requests.get(build_uri('user/emails'), auth=auth)
    print response.text

```

```
import base64
base64.b64decode('')  #这里的字符串是一般的基础验证的利用base64编码的字符串
# 使用b64decode就可以查看编码前的信息了
```

##### requests学习工具

```
gunicorn==19.6.0
httpbin==0.5.0
requests==2.12.5

```
启动学习使用的服务器:
gunicorn httpbin:app
