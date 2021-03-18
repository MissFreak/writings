---
title: "如何在curl和python中使用API"
categories:
- 网站开发
- flask
tags:
- api
---

除了自己构建API，我们也可能需要使用其他网站提供的API。
<!-- more -->

# 在curl中使用API

我们以学术检索网站[semantic scholar](https://api.semanticscholar.org/)为例，查找某论文的信息，并保存为JSON文件[^1]。

```
curl https://api.semanticscholar.org/v1/paper/10.1038/nrn3241 > paper1.json
```

但此时的json数据结构并不整洁，我们用python格式化json字符串并保存在paper2.json[^2]。

```python
python -m json.tool paper1.json paper2.json
```

或者一步到位[^3]：

```
curl https://api.semanticscholar.org/v1/paper/10.1038/nrn3241 | python -mjson.tool > paper3.json
```

如果仅仅是想显示为Pretty Print打印出来，把后面的filename.json去掉即可。

# 用python获取API数据

依旧是这个例子，我们将返回的数据转成字典格式[^4]：

```python
import json
import requests

response = requests.get("https://api.semanticscholar.org/v1/paper/10.1038/nrn3241")
response_dic = response.json() # if the result was written in JSON format, if not it raises an error
```

更多requests相关参考[^5]。



**参考文献：**

[^1]:[curl 的用法指南](https://www.ruanyifeng.com/blog/2019/09/curl-reference.html)

[^2]: [python json官方文档](https://docs.python.org/3/library/json.html)

[^3]: [在命令行上漂亮地打印JSON的最佳方法](https://skorks.com/2013/04/the-best-way-to-pretty-print-json-on-the-command-line/)
[^4]: [透过curl、Python、Postman 来Request API](https://jzchangmark.wordpress.com/2016/06/12/%E9%80%8F%E9%81%8E-curl%E3%80%81python%E3%80%81postman-%E4%BE%86-request-api/)
[^5]: [python请求](https://www.w3schools.com/python/ref_requests_response.asp)
[^6]: [Github API](https://docs.github.com/en/rest)
[^7]: [干货贴！Github上超过百赞的爬虫脚本合集](https://blog.csdn.net/raymondsage/article/details/87883661?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-4.control&dist_request_id=&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-4.control)
[^8]: [有哪些好玩的免费的API接口?](https://www.zhihu.com/question/32225726)