---
title: "基础：用flask搭建RESTful API"
categories:
- 网站开发
- flask
tags:
- flask
- api
---

[Flask](https://flask.palletsprojects.com/en/1.1.x/)是目前发展最迅速的 Python 框架之一，它是一个微型的 Python 开发的 Web 框架，基于[Werkzeug](https://www.oschina.net/p/werkzeug) WSGI工具箱和[Jinja2](https://www.oschina.net/p/jinja) 模板引擎。以下是用flask来构建 RESTful API  的全过程实录，力求完整、准确、无误地记录每个操作步骤与细节。我用的是windows系统，但我也会提到其他系统的操作。
<!-- more -->
# 环境搭建与工具安装

首先依然是创建虚拟环境并安装 flask 和 [flask-restful](https://flask-restful.readthedocs.io/en/latest/) package。


```
mkvirtualenv flask-api
pip install flask
pip install flask-restful
```
# 使用flask搭建API
## 最简单的flask应用

新建一个python脚本，linux用touch，windows操作如下：

```python
workon flask-api # 激活虚拟环境
D:
type nul>hello.py # 新建文件
atom hello.py # 用atom编辑器打开
```

粘贴以下代码：

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')  # 把Flask对象中的route()函数作为一个装饰器，增强该函数的功能
def hello_world():
    return 'Hello, World!'
```

再打开 Command Prompt 输入：

```
set FLASK_APP=hello.py
set FLASK_ENV=development
flask run
```

按下快捷键Alt+Shift+=左右分屏，输入：

```bash
curl http://127.0.0.1:5000/
curl -v http://127.0.0.1:5000/
```

输出如下：

```
...
< Content-Type: text/html; charset=utf-8
< Content-Length: 13
...
<
Hello, World!* Closing connection 0
```

我们发现返回的是html类型，而不是通常从 RESTful API 获得的 json 文件，于是修改hello.py的代码：

```python
from flask import Flask, jsonify
app = Flask(__name__)

@app.route('/')
def hello_world():
    return jsonify({'about': 'Hello, World!'})

#或者：
@app.route('/')
def hello_world():
    return jsonify(about='Hello, World!')
```

调试模式下，服务器会监测你的代码更新并自动加载，所以无须重启才看到效果。这时curl得到json file：

```
...
< Content-Type: application/json
< Content-Length: 31
...
<
{
  "hello": "Hello, World!"
}
* Closing connection 0
```

## 通过URL传递参数

我们可以通过url传递参数给flask api，例如`/<int:num>/`

将hello.py修改为：

```python
from flask import Flask, jsonify, request

app = Flask(__name__)

@app.route('/multi/<int:num>', methods=['GET']) # methods一般默认为GET
def get_multiply10(num):
    return jsonify({'result': num*10}) # 返回数字乘以10后的结果


if __name__ == '__main__':
    app.run(debug=True)
```

然后在cmd输入：

```
curl http://127.0.0.1:5000/multi/10
```

我们得到10乘以10后的结果：

```
{
  "result": 100
}
```

当然，我们也可以在浏览器内访问以下地址：

```
http://127.0.0.1:5000/multi/10
```

## 使用POST方法

这一小节我们将使用POST method。

在hello.py中加入以下代码：

```python
@app.route('/', methods=["GET", "POST"])
def index():
    if (request.method == "POST"):
        some_json = request.get_json()
        return jsonify({'You sent': some_json}), 201 # 如果是post则返回post内容
    else:
        return jsonify({"about": "Hello World!"}) # 如果是get则返回hello world
```

现在，我们可以发送如下curl请求并返回POST的内容：

```
curl -H "Content-Type: application/json" -X POST -d '{"name":"Example","email":"example@example.com"}' http://127.0.0.1:5000/
```

注意：Windows的命令行不支持单引号、且需要将双引号转义，所以需要改成：

```
curl -H "Content-Type: application/json" -X POST -d "{\"name\":\"Example\",\"email\":\"example@example.com\"}" http://127.0.0.1:5000/
```

因此，Windows用户建议在git bash上完成以上操作。

# 使用flask-restful搭建API

我们把hello.py修改为：

```python
from flask import Flask, request
from flask_restful import Resource, Api

app = Flask(__name__)
api = Api(app)

class HelloWorld(Resource):
    def get(self):
        some_json = request.get_json()
        return {'You sent': some_json}, 201

class Multi(Resource):
    def get(self, num):
        return {'result': num*10}

api.add_resource(HelloWorld, '/')
api.add_resource(Multi, '/multi/<int:num>')
```

以上代码实现完全相同的功能，不过看起来更简洁和舒服。



参考教程：

https://www.youtube.com/watch?v=s_ht4AKnWZg

参考文献：

https://blog.csdn.net/weixin_41010198/article/details/85230424#API_3
