---
title: "纯文本文件的处理"
categories:
- 代码
- 文本处理
tags:
- txt
---
# 获取文章目录

```python
import os
import re
import itertools
file_path = r'D:\project\hexo-final\source\_posts'
lst = os.listdir(file_path)
post_list = []

# obtain the titles and categories of all posts
for filename in lst:
	# make sure it is a markdown file
	if filename[-3:] == '.md':
		with open(os.path.join(file_path, filename), encoding="utf-8") as f:
			s = f.read()

		# create a dict that stores attributes: title and category_1 and category_2
		post_dict = {}

		# get the title
		match_title = re.search(r'(?<=title: ).*(?=\n)', s)
		if match_title:
			title = match_title.group().strip('"')
		else:
			title = filename

		# convert into linked title
		linked_title = '[{}](https://github.com/MissFreak/writings/blob/main/{})'.format(title, filename)

		# get the categories
		match_categories = re.search(r'categories:\n- (.*)\n- (.*)\n', s)
		try:
			category_1 = match_categories.group(1)
			category_2 = match_categories.group(2)
		except:
			category_1 = '未分类'
			category_2 = '未分类'

		# save into a list
		post_dict['title'] = linked_title
		post_dict['category_1'] = category_1
		post_dict['category_2'] = category_2
		post_list.append(post_dict)

# sort and group by the first category
get_first_category = lambda dct: dct.get('category_1')
group_1 = itertools.groupby(sorted(post_list, key=get_first_category), get_first_category)
num_post = len(post_list)

content = '# 目录\n这里是我的博客，目前共有{}篇文章：\n'.format(num_post)
for k1,v1 in group_1:
	# add the first category into content
	content += ('## '+k1+'\n')
	# sort and group by the first category
	get_second_category = lambda dct: dct.get('category_2')
	group_2 = itertools.groupby(sorted(v1, key=get_second_category), get_second_category)
	for k2,v2 in group_2:
		# add the second category into content
		if k2 != '未分类':
			content += ('- _'+k2+'_\n')
			for i in v2:
				# add the title into content
				content += ('\t- '+i['title']+'\n\n')
		else:
			for i in v2:
				# add the title into content
				content += ('- '+i['title']+'\n\n')

# write the content into md file
with open(os.path.join(file_path, 'README.md'), 'w', encoding='utf-8') as f:
	f.write(content)
```


# 正则表达式
```python
import re
file_path = r'D:\books\Psychology_of_Language.txt'
with open(file_path, encoding='utf-8') as f:
    txt_string = f.read()
pattern = re.compile(r'第一 初见.*第二 再见', re.DOTALL)
cha1 = re.search(pattern, txt_string).group()
print(re.findall(r'蓝色', cha1))

pattern = re.compile(r'第一 初见[^蓝色]*(蓝色)*.*第二 再见', re.DOTALL)
match = re.search(pattern, txt_string)
if match:
    print(match.groups())
```
<!-- more -->
# 分离章节

```python
import re
file_path = r'D:\books\Psychology_of_Language.txt'
with open(file_path, encoding='utf-8') as f:
    txt_string = f.read()
pattern = re.compile(r'\ng\n')
chapters = pattern.split(txt_string)[6:]

cha8 = chapters[7]
cha8 = re.sub(r'\nn\n', '', cha8)
cha8 = re.sub(r'\n{2,}', '\n', cha8)
cha8 = re.sub(r'-', '', cha8)
# print(repr(cha8))
print(cha8)
```

# 去除字符

```python
ss = '我的电话是18827038663，也是微信号，\n 请加入，谢谢\n\n\n'
print(ss.strip('\n'))
ss = '我的电话是18827038663，也是微信号，\n 请加入，谢谢\n\n\n'
print(ss.replace('\n', ''))
ss = '我的电话是18827038663，也是微信号，请加入，谢谢啦啦嗯'
print(ss.strip('嗯啦'))
```

# 表格：[pytablewriter](https://github.com/thombashi/pytablewriter/)

```
MarkdownTableWriter, ExcelXlsxTableWriter, UnicodeTableWriter, JavaScriptTableWriter, JsonTableWriter, HtmlTableWriter
```

先获得matrix：

```python
with open('tempo.txt') as f:
	s = f.read()
matrix = [i.split(': ') for i in s.split('\n')] # 行分隔符和列分隔符
print(matrix)

with open('tempo2.txt') as f:
	s = f.read()
matrix = [i.split(' ', 1) for i in s.split('\n')] # split on first occurrence
print(matrix)
```

然后生成相应的表格：

```python
import pytablewriter
writer = pytablewriter.MarkdownTableWriter()
writer.value_matrix = matrix
writer.write_table()
```

## markdown表格

```python
import spacy
import numpy as np
# s = spacy.explain('``')
# print(repr(s))
lst1 = '`$`, `''`, `,`, `-LRB-`, `-RRB-`, `.`, `:`, `ADD`, `AFX`, `CC`, `CD`, `DT`, `EX`, `FW`, `HYPH`, `IN`, `JJ`, `JJR`, `JJS`, `LS`, `MD`, `NFP`, `NN`, `NNP`, `NNPS`, `NNS`, `PDT`, `POS`, `PRP`, `PRP$`, `RB`, `RBR`, `RBS`, `RP`, `SYM`, `TO`, `UH`, `VB`, `VBD`, `VBG`, `VBN`, `VBP`, `VBZ`, `WDT`, `WP`, `WP$`, `WRB`, `XX`'.split(',')
lst2 = [spacy.explain(i.strip(' `')) for i in lst1]
# print(lst2)

matrix = np.column_stack((lst1, lst2)).tolist()
# print(type(matrix))
# print(matrix)
import pytablewriter
writer = pytablewriter.MarkdownTableWriter()
writer.value_matrix = matrix
writer.write_table()
```

## csv

```python
import pytablewriter

def main():
    writer = pytablewriter.CsvTableWriter()
    writer.headers = ["int", "float", "str", "bool", "mix", "time"]
    writer.value_matrix = [
        [0,   0.1,      "hoge", True,   0,      "2017-01-01 03:04:05+0900"],
        [2,   "-2.23",  "foo",  False,  None,   "2017-12-23 45:01:23+0900"],
        [3,   0,        "bar",  "true",  "inf", "2017-03-03 33:44:55+0900"],
        [-10, -9.9,     "",     "FALSE", "nan", "2017-01-01 00:00:00+0900"],
    ]

    writer.write_table()

if __name__ == "__main__":
    main()
```

## 其他格式

[JSON](https://pytablewriter.readthedocs.io/en/latest/pages/examples/table_format/text/json.html)

[HTML](https://pytablewriter.readthedocs.io/en/latest/pages/examples/table_format/text/html.html)
