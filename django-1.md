---
title: "Django网站开发全过程实录-1"
categories:
- 网站开发
- django
tags:
- django
- web
---

网站一般需要实现三种基本功能：**连接数据库、处理用户请求、页面设计的删改**。Django的优势在于将这些功能设计成独立的模块，形成一套web框架。利用Django框架开发网站，能让我们专注于编写应用程序而无需重新造轮子。
<!-- more -->
Django 采用了 **MVT 的软件设计模式**，即**模型**（Model）、**视图**（View）和**模板**（Template）。这种设计模式的优势在于**各个组件松散结合**，每个APP应用都有明确的目的，并且可独立更改而不影响其它部分。如此，使得页面设计与业务逻辑互不影响。同时，Django是一套出色的**动态内容管理系统**，擅长动态提供数据库驱动的信息。

以下是我使用Django 3.1.7搭建网站过程的实录，力求完整、准确、无误地记录每个操作步骤与细节。

## **环境搭建与工具安装**

> *参考：*[*https://stormsha.com/article/2026/*](https://stormsha.com/article/2026/)

我们需要在合适的目录内创建一个**虚拟环境**（用virtualenv, virtualenvwrapper皆可，参考[virtualenvwrapper的使用](https://blog.csdn.net/a200822146085/article/details/89048172)），我给它取名为webdev。

在该虚拟环境，安装**django**和**psycopg2**工具包（用于管理PostgreSQL数据库）。

```
pip install virtualenv, virtualenvwrapper-win
mkvirtualenv webdev
workon webdev
pip install django
pip install psycopg2
pip list
pip freeze

# 如果需要退出或删除虚拟环境
deactivate
rmvirtualenv webdev
```

**安装数据库：**

Django支持四种数据库：PostgreSQL、SQLite 3、MySQL、Oracle。

我选择PostgreSQL，它比MySQL更适合Django，Django的创建者如是说：

> 如果您不受任何遗留系统的束缚，并且可以自由选择数据库后端，那么我们建议您使用PostgreSQL，它可以在成本、功能、速度和稳定性之间取得很好的平衡。（《 Django权威指南》第15页）

PostgreSQL的安装步骤参考：https://www.runoob.com/postgresql/windows-install-postgresql.html

打开后设置语言为中文，然后关闭。

## **创建项目（project）**

下面在刚刚的虚拟环境webdev内创建一个项目mysite（你可以选择任意其他名字），项目是我们所建立的网站上所有应用程序的集合，并共用一套数据库配置。

```
cd webdev
django-admin startproject mysite
```

我们看到新建了一个文件夹mysite及下面的子文件夹mysite/mysite：

```
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        asgi.py
        wsgi.py
```

我们可以把子文件夹mysite/mysite视为整个项目的配置，其中的settings.py和urls.py这两个文件是我们以后需要经常修改的。

## **启动服务器（server）**

启动服务器，服务器会监测你的代码更新并自动加载，所以无须重启才看到效果：

```
cd mysite
python manage.py runserver
```

在settings.py内将语言改成中文，时区改为上海：

```
LANGUAGE_CODE = 'zh-hans'

TIME_ZONE = 'Asia/Shanghai'
```

刷新浏览器看到中文页面。

我们可以指定服务器的端口和IP地址。比如，把地址设为自己的IP地址（例如192.168.1.110）或0.0.0.0，让联网的其他计算机可见：

```
python manage.py runserver 0.0.0.0:8000
```

使用Windows的用户用ipconfig命令获取本地网络中的IP 地址，然后复制到setting.py中，比如我是这个：

```
ALLOWED_HOSTS = ['192.168.1.110']
```

于是，在其他电脑或手机浏览器打开 http://192.168.1.110:8000/ 就可以访问啦！完美！不过网站还在开发中，就不要随便开放共享啦~

## **创建应用程序（APP）**

**项目和应用的区分：**

- **应用**是用于执行某项具体操作的程序，**项目**是特定网站的配置和应用程序的集合。
- **多对多的关系**：一个项目可以包含多个应用程序，一个应用程序可以用在多个项目中。

应用放在任意路径都可以，但我们一般放在**manage.py文件相同的目录**中，与mysite子文件夹平行。在这里我创建一个成语检索的app，名为idiom：

```
python manage.py startapp idiom
```

看看这个应用程序下有哪些文件：

```
idiom/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```

## **编写视图（views）**

下面为这个应用程序idiom添砖加瓦，分为三个步骤：

1. 创建视图函数
2. 将视图函数映射到APP的urls
3. 将APP中的urls连入网站的根urls

这样看逻辑可能更清晰：视图函数 --> APP的urls --> 网站的urls

## **创建视图函数**

打开文件idiom/views.py ，加入以下Python代码，**创建index视图函数**：

```
from django.http import HttpResponse

def index(request):
    return HttpResponse("Hello, world. You're at the idiom index.")
```

## **映射到APP的urls**

要调用该视图，我们要将其映射到URL，为此，我们需要添加一个URL配置（URLconf）。**URLconf**相当于网站的目录，也就是**URL模式与视图函数之间的映射表**。

我们在idiom应用的目录下创建一个名为urls.py的文件，windows操作如下：

```
type nul>urls.py
```

看看现在的应用目录：

```
idiom/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    urls.py
    views.py
```

然后在idiom/urls.py这个空文件中加入以下代码，将index视图映射到APP的url模式：

```
from django.urls import path

from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
```

## **连入网站的urls**

下一步是将根URLconf（mysite/urls.py）指向idiom.urls模块，使得网站域名连接到app的url。我们打开mysite/urls.py，修改为：

```
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('idiom/', include('idiom.urls')),
    path('admin/', admin.site.urls),
]
```

该include()功能允许引用其他URLconf，这样我们就将刚刚创建的index视图连接到了网站的URLconf。

也就是说，目前我们可以打开两个网址：

[http://example.com/idiom/](http://127.0.0.1:8000/idiom/)

[http://example.com/admin/](http://127.0.0.1:8000/idiom/)

注意idiom和admin在引用URL模式时的区别：除了admin.site.urls（用于管理后台），我们引用其他URL模式时，都应使用include()。

最后，验证下是否正常运行：

```
python manage.py runserver
```

打开http://127.0.0.1:8000/idiom/ ，可以看到 Hello, world. You're at the idiom index. 这行文字。

## **URLconf的工作原理**

Django允许我们根据需要设计每个应用程序的URL，通过创建**URLconf**（URL配置）。

在 idiom和mysite文件夹下的urls.py中，我们都使用了[path()](https://docs.djangoproject.com/en/3.1/ref/urls/#django.urls.path)函数，这个函数有两个必需的参数 route和view。

path（*route*，*view*，*kwargs = None*，*name = None*）

- route是包含URL模式的字符串，比如目前我们有idiom/和admin/。在处理请求时，Django从第一个模式开始，沿列表的顺序，将请求的URL（域名后的部分）与每个模式进行比较，直到找到匹配的URL。这个字符串支持用尖括号匹配和捕获URL的一部分并将其作为关键字参数发送到视图，
- view就是指定的视图函数，也可以是一个[django.urls.include()](https://docs.djangoproject.com/en/3.1/ref/urls/#django.urls.include)。kwargs参数允许我们将其他参数传递给视图函数。
- name不是必须的，但是命名URL的好处是便于在Django中的其他地方（尤其是在模板内部）明确地引用它。

参考：[django.urls functions for use in URLconfs](https://docs.djangoproject.com/en/3.1/ref/urls/#django.urls.path)
