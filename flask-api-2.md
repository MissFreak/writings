---
title: "部署Flask开发的API到Heroku"
categories:
- 网站开发
- flask
tags:
- flask
- api
---


上篇文章我们介绍了如何用flask开发简单的web api，下面我们把它部署到heroku上，方便更多人使用。
<!-- more -->

**步骤总结：**

- 注册[Heroku帐号](https://signup.heroku.com/)

- 下载客户端

- 在本地命令行登录
  
  - 如果出现IP Address Mismatch，复制并粘贴`heroku login -i`到终端，用邮箱密码登录
  
- 创建应用
  
  - ```
    heroku apps:create flask-microblog
    ```
  
- 初始化
  
  - ```
    mkdir flask-api
    cd flask-api
    git init
    ```
  
- 编写应用代码

  - run.py
  - requirements.txt
  - `echo web: gunicorn run:app > Procfile`
    - `web: gunicorn <filename>:<main method name>`
  - 用`tree/F`检验上述文件是否齐全
  
- 部署应用

  - 关联github，自动部署

  - ```
    git add .
    git commit -m "Initialize repo"
    git push -u origin master
    ```

- 访问应用地址：https://nlp-ch.herokuapp.com/



**参考文献：**

官方教程：

https://devcenter.heroku.com/articles/getting-started-with-python

以及这些博客：

https://noviachen.github.io/posts/b4cb2e1c.html

https://wizardforcel.gitbooks.io/the-flask-mega-tutorial-2017-zh/content/docs/18.html

http://www.bjhee.com/flask-heroku.html

windows上部署参考（建议使用git bash）：https://caijialinxx.github.io/2018/07/25/deploy-on-heroku/

git bash创建并编辑文件参考：https://blog.csdn.net/qq_34289537/article/details/53994070

windows cmd常用命令参考：https://www.jianshu.com/p/80c3ac7bea8f

上传文件到github: https://www.jianshu.com/p/5227f837070b
