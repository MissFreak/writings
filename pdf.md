---
title: PDF文件处理大全
categories:
- 代码
- 文本处理
tags:
- pdf
---
# Pdfplumber：PDF文件预处理

## 用pdfminer把pdf转为txt

```python
from pdfminer.high_level import extract_text
file_path = r'D:\pdf-file\Psychology_of_Language.pdf'
text = extract_text(file_path, page_numbers=range(33,35))
```
<!-- more -->
`pdfminer.high_level.``extract_text`（*pdf_file*，*password =''*，*page_numbers = None*，*maxpages = 0*，*caching = True*，*codec ='utf-8'*，*laparams = None* ）

参考：https://pdfminersix.readthedocs.io/en/latest/reference/highlevel.html#api-extract-text

## 更精确的转换（除去换行符）
### 少量文件转换

#### 先把pdf转换为word

转换工具：https://pdf2doc.com/zh/

#### 然后把word转为txt

文件多的话用多线程PDF转Word：https://github.com/python-fan/pdf2word

### 批量转换

还不知道，参考：https://www.zhihu.com/question/357994254

```python
import pdfplumber
import os
file_path = 'book.pdf'
pdf = pdfplumber.open(file_path)
start_page = 34
end_page = 35
text_string = ''
for page_num in range(start_page-1, end_page):
    text_string += pdf.pages[page_num].extract_text()+' '
with open('book.txt', 'w') as f:
    f.write(text_string)
```

## 把表格保存为csv

```python
import pdfplumber
import os
file_path = 'book.pdf'
pdf = pdfplumber.open(file_path)

chars = []
for page in pdf.pages[7:166]:
    for char in page.chars:
        chars.append(char)
width_unique = set([char['height'] for char in chars])
print('The unique heights: '+str(width_unique))
```
