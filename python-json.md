---
title: "用python处理json数据"
categories:
- 代码
- 文本处理
tags:
- json
---
> [JSON](https://baike.baidu.com/item/JSON)([JavaScript](https://baike.baidu.com/item/JavaScript) Object Notation, JS 对象简谱) 是一种轻量级的数据交换格式。它基于 [ECMAScript](https://baike.baidu.com/item/ECMAScript) (欧洲计算机协会制定的js规范)的一个子集，采用完全独立于编程语言的文本格式来存储和表示数据。简洁和清晰的层次结构使得 JSON 成为理想的数据交换语言。 易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率。——百度百科

编码和解码JSON数据的过程相当于把同一样东西翻译成中文和日语。

```python
json_str = {"name":"example","age":18}
# 序列化json
json_str = json.dumps(params, sort_keys=True)
# 反序列化json
dict_json = json.loads(json_str)
```
<!-- more -->
# 序列化JSON

## 将Python对象转换为JSON

| Python                 | JSON格式 |
| ---------------------- | -------- |
| `dict`                 | `object` |
| `list`， `tuple`       | `array`  |
| `str`                  | `string` |
| `int`，`long`，`float` | `number` |
| `True`                 | `true`   |
| `False`                | `false`  |
| `None`                 | `null`   |

```python
data = {
    "president": {
        "name": "Zaphod Beeblebrox",
        "species": "Betelgeusian"
    }
}
type(data)
#<class 'dict'>

# 保存为json文件
with open("data_file.json", "w") as write_file:
    json.dump(data, write_file)

# 或者将序列化的JSON数据写入python字符串对象
json_string = json.dumps(data)
# <class 'str'>
```

## 一些有用的关键字参数

```python
json.dumps(data, indent=4, sort_keys = True)
```

indent定义缩进的级别；如果*sort_keys*为true，则字典的输出将按key排序。


# 反序列化JSON：读取JSON数据

## 将JSON编码的数据转换为Python对象

| JSON            | Python  |
| --------------- | ------- |
| `object`        | `dict`  |
| `array`         | `list`  |
| `string`        | `str`   |
| `number` (int)  | `int`   |
| `number` (real) | `float` |
| `true`          | `True`  |
| `false`         | `False` |
| `null`          | `None`  |

## 从文件中读取

```python
import json
with open("data_file.json", "r", encoding="utf-8") as read_file:
    data = json.load(read_file)
type(data)
# <class 'list'>
type(data[0])
# <class 'dict'>
```

## 从字符串创建

```python
json_string = """
{
    "researcher": {
        "name": "Ford Prefect",
        "species": "Betelgeusian",
        "relatives": [
            {
                "name": "Zaphod Beeblebrox",
                "species": "Betelgeusian"
            }
        ]
    }
}
"""
data = json.loads(json_string)
```

## 从API获取

```python
import json
import requests
response = requests.get("https://jsonplaceholder.typicode.com/todos")
todos = json.loads(response.text)
todos == response.json()
```

这里的伪json数据适合用来练习。

# 遍历JSON字符串

```
items = data.items()
for key, value in items:
    print(str(key) + '=' + str(value))
```

**参考文献：**

https://realpython.com/python-json/
