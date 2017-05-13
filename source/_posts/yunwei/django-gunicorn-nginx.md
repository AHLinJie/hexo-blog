---
layout: post
title: Django&Gunicorn&Nginx
date: 2016-09-07
category: 运维
tag: deploy
---

**摘要:**
部署项目有很多的选择,有些比较合适有些比较不合适,这个要看个人经验,这个组合是我以前
的一个项目上面使用的.应该可以用.下面的内容纯属自己测试使用的没有正式上线.


#### Install software

1. sudo apt-get update
2. sudo apt-get install python-pip python-dev libpq-dev postgresql postgresql-contrib nginx
3. sudo pip install virtualenv

#### Create DATABASE
1. sudo su - postgres
2. psql
3. CREATE DATABASE myproject;
4. CREATE USER myprojectuser WITH PASSWORD 'password';
5. GRANT ALL PRIVILEGES ON DATABASE myproject TO myprojectuser;
6. \q
7. exit

#### Create Django Project

1. mkdir ~/myproject
2. cd ~/myproject
3. virtualenv myprojectenv
4. source myprojectenv/bin/activate

#### Install requriement
1. pip install django gunicorn psycopg2
2. django-admin.py startproject myproject .

#### Config Django Settings
1. nano myproject/settings.py
2. edit under codes

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'myproject',
        'USER': 'myprojectuser',
        'PASSWORD': 'password',
        'HOST': 'localhost',
        'PORT': '',
    }
}
STATIC_ROOT = os.path.join(BASE_DIR, "static/")
```


#### Migrate Data
1. cd ~/myproject
2. ./manage.py makemigrations
3. ./manage.py migrate

#### Create Django User & Run Django Project
1. ./manage.py createsuperuser
2. ./manage.py collectstatic
3. ./manage.py runserver 0.0.0.0:8000

#### Bind Gunicorn Interface Directly
1. cd ~/myproject
2. gunicorn --bind 0.0.0.0:8000 myproject.wsgi:application

#### Config Gunicorn 
1. sudo nano /etc/init/gunicorn.conf
2. edit under code
```
description "Gunicorn application server handling myproject"

start on runlevel [2345]
stop on runlevel [!2345]

respawn
setuid jishu_linjie
setgid www-data
chdir /home/jishu_linjie/studyspace/django-stu/myproject/myproject
exec /home/jishu_linjie/studyspace/django-stu/myproject/myprojectenv/bin/gunicorn --workers 3 --bind 192.168.1.31:8000 myproject.wsgi:application(myprojectenv)jish
```


3. sudo service gunicorn start

#### Config Nginx 
1. sudo nano /etc/nginx/sites-available/myproject
2. edit code under
```
server {
       listen 8001;
       server_name 192.168.1.31;
       
       location = /favicon.ico { access_log off; log_not_found off; }
       location /static/ {
       		root /home/jishu_linjie/studyspace/django-stu/myproject;
	}

       location / {
       		include proxy_params;
		proxy_pass http://192.168.1.31:8000;
       }

```
3. sudo ln -s /etc/nginx/sites-available/myproject /etc/nginx/sites-enabled
4. sudo nginx -t
5. sudo service nginx restart






