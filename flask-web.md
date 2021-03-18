---
title: 一个人开发信息检索与抽取网站的全过程
categories:
- 网站开发
- flask
tags:
- web
---
# 我的网站开发学习

我不会使用css或js文件，因为我只专注于python后端开发，前端基本都省去，只用bootstrap，但是要精通bootstrap哟！

计划：

1. 完成检索功能，包括错误检测。检索成语、句子、词语等等。
2. 其他小插件的完善
3. 重点是毕业论文

<!-- more -->

# 搜索框

首先我们把boostrap加入html页面，直接使用[BootstrapCDN](https://www.bootstrapcdn.com/)跳过下载，将Bootstrap编译的CSS和JS的缓存版本交付给我们的项目。我们使用最简单的主题即可，也可以使用其他主题（比如[minty](https://bootswatch.com/minty/)）。

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
</head>
<body>
	<form class="form my-2 my-lg-0">
        <input class="form-control mr-sm-2" type="text" placeholder="Search">
    	<button class="btn btn-secondary my-2 my-sm-0" type="submit">Search</button>
    </form>
</body>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js" integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
</html>
```



```python
# -*- coding:utf-8 -*-
import json
with open("idiom.json", "r", encoding="utf-8") as read_file:
    idiom_list = json.load(read_file)

idiom_dict = {}
for idiom in idiom_list:
    idiom_dict[idiom['word']] = idiom

with open("idiom_dict.json", "w", encoding="utf-8") as write_file:
    json.dump(idiom_dict, write_file)
print('成功将原列表转换为key为成语的字典！')
```





查看源代码，







参考：

https://www.bootdey.com/snippets/view/Search-Results#html

[百度搜索框](https://blog.csdn.net/star_xing123/article/details/101271925)

[怎么在HTML中加入css样式？ - html中文网](https://www.html.cn/qa/css3/12786.html)

[Bootstrap Introduction](https://getbootstrap.com/docs/4.3/getting-started/introduction/)

[Free themes for Bootstrap](https://bootswatch.com/)
