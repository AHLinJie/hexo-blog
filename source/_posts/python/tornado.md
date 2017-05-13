---
layout: post
title: tornado基础
category: webframework
tag: tornado
date: 2017-2-14
---

**摘要：**
tornado是一个异步非阻塞的web开发框架，这里添加一些基础的代码，学习ing.
不断更新吧。模板和django的还是很相似的。
[github](https://github.com/tornadoweb/tornado)
[中文基础教程](docs.pythontab.com/tornado/introduction-to-tornado)

##### web requesthandler

1. hello.py
```
#coding=utf-8
import textwrap
import tornado.options
import tornado.ioloop
import tornado.web

from tornado.options import define, options


define("port", default=8888, help='run on the given port', type=int)


class MainHandler(tornado.web.RequestHandler):
    def get(self):
        self.write("Hello, world")


class StoryHandler(tornado.web.RequestHandler):
    def get(self, story_id):
        self.write("You request the story id is: %s" % story_id)
        print self.get_argument('user_name', 'nobody')  # ?user_name=abcdef


class ReverseHandler(tornado.web.RequestHandler):
    def get(self, input_str):
        self.write("reserves result is: %s" % input_str[::-1])


class WrapHandler(tornado.web.RequestHandler):
    def get(self):
        self.write('this is get method')

    # http post 方法
    def post(self):
        text = self.get_argument('text')
        print 'text is:', text
        width = self.get_argument('width', 40)
        print 'width is:', width
        self.write(textwrap.fill(text, int(width)))


class FrobHandler(tornado.web.RequestHandler):
    def get(self, frob_id):
        # frob = retrive_from_db(frob_id)
        # self.write(frob.serialize()) 
        self.write('after')
        
    # http head 方法
    def head(self, frob_id):
        if frob_id:
            self.set_status(200)  # 使用set_status 设置请求头
        else:
            self.set_status(404)


class RewriteErrorHandler(tornado.web.RequestHandler):
    def get(self):
        self.write('get example')

    # 重写错误相应方法
    def write_error(self, status_code, **kwargs):  # 注意这里的参数
        self.write('this is rewrite the write_error func')


def make_app():
    return tornado.web.Application([
        (r"/", MainHandler),
        (r"/story/([0-9]+)", StoryHandler),
        (r"/wrap", WrapHandler),
        (r"/reverse/(\w+)", ReverseHandler),
        (r"/frob/([0-9]+)", FrobHandler),
        (r"/werror", RewriteErrorHandler),
    ])

if __name__ == "__main__":
    tornado.options.parse_command_line()
    app = make_app()
    app.listen(options.port)
    tornado.ioloop.IOLoop.instance().start()
```

##### templates
1. 后台代码
```
#coding=utf-8
import os.path

import tornado.httpserver
import tornado.ioloop
import tornado.options
import tornado.web

from tornado.options import define, options


define('port', default=8888, help='run on the give port', type=int)


class IndexHandler(tornado.web.RequestHandler):

    def get(self):
        self.render('index.html')


class PoemHandler(tornado.web.RequestHandler):

    def post(self):
        noun1 = self.get_argument('noun1')
        noun2 = self.get_argument('noun2')
        noun3 = self.get_argument('noun3')
        verb = self.get_argument('verb')
        self.render('poem.html', 
                    roads=noun1, 
                    wood=noun2, 
                    made=verb,
                    difference=noun3)

# 模板中使用模块
class HelloModule(tornado.web.UIModule):

    def render(self):
        return '<h2>this is a uimodule render</h2>'
    

def make_app():
    return tornado.web.Application(
            [
                (r'/', IndexHandler),
                (r'/poem', PoemHandler),
            ],
            template_path=os.path.join(os.path.dirname(__file__), 'templates'),
            static_path=os.path.join(os.path.dirname(__file__), 'static'),
            ui_modules = {'hello': HelloModule}, ## 模板中使用module
        )


if __name__ == '__main__':
    tornado.options.parse_command_line()
    app = make_app()
    app.listen(options.port)
    tornado.ioloop.IOLoop.instance().start()
```
2. templates/poem.html
```
<!DOCTYPE html>
<html>
    <head><title>Poem Maker Pro</title></head>
    <body>
        <h1>Your poem</h1>
	<p>
	Two {{roads}} diverged in a {{wood}}, and I—<br>
	I took the one less travelled by,<br>
	And that has {{made}} all the {{difference}}.
	</p>
    </body>
</html>
```
4. template/index.html
```
<!DOCTYPE html>
<html>
    <head>
        <title>Poem Maker Pro</title>
        <script type="text/javascript" src="{{static_url("console.js") }}"></script>
    </head>
    <body>
        <h1>Enter terms below.</h1>
        <form method="post" action="/poem">
        <p>Plural noun<br><input type="text" name="noun1"></p>
        <p>Singular noun<br><input type="text" name="noun2"></p>
        <p>Verb (past tense)<br><input type="text" name="verb"></p>
        <p>Noun<br><input type="text" name="noun3"></p>
        <input type="submit">
        </form>


        <div>
            <h1>模板中使用模块</h1>
            {% module hello() %}
        </div>
    </body>
</html>
```
5. console.js
```
console.log('console active')
```
