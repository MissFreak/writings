---
title: "正则表达式进阶"
categories:
- 自然语言处理
- 正则表达式
tags:
- regex
---

至此，我们已经建立了正则表达式的思维框架（参见「[正则表达式入门](http://mp.weixin.qq.com/s?__biz=MzIzMDY0NDQ1Ng==&mid=2247484919&idx=1&sn=7309a9bf1be78ea3250838724ebaa81c&chksm=e8b10a70dfc683666f6d87e6fda6f51863dbaf565ac522b6d01d80059c01a821af70bc279438&scene=21#wechat_redirect)」），以及如何用python中的re模块编译正则表达式来匹配文本（参见「[Python实操篇](https://mp.weixin.qq.com/s/ibNb0rOSnBr4YC0PzCyIBA)」）。尽管此时我们已经可以流畅地编写和使用它，但我们需要一些进阶知识，让我们的正则表达式更准确和精练。本文使用的语言依然是python。
<!-- more -->

**全文概览：**

1.分组与捕获：`MatchObject.group()`的奥秘

2.四种类型的环视：匹配位置，而非匹配文本

3.贪婪与非贪婪：匹配优先 VS 忽略优先

# 分组与捕获

在「[Python实操篇](https://mp.weixin.qq.com/s/ibNb0rOSnBr4YC0PzCyIBA)」我们讲到`MatchObject`的方法`group()`可以返回匹配的整个字符串，但这并不全面。本节，我们将谈谈括号的一个关键功能：捕获（capturing）。

**捕获**就是用括号提取文本以便后续访问，我们可以把这个过程想象成“闰土捕鸟”，把每个括号里的鸟儿都抓起来放进“笼子”。


而捕获进所有“笼子”里的内容，正是通过`MatchObject.groups()`访问。

```python
re.search(r"(\w+) (\w+)", "John Smith").groups()
# 输出：('John', 'Smith')
```

我们可以通过索引来访问每个“笼子”，`group(0)`返回整个正则表达式匹配的文本，相当于编号为0的隐式捕获，而`group(1)`、`group(2)`则按顺序返回显式捕获分组。

```python
m = re.search(r"(\w+) (\w+)", "John Smith")
m.group(0) # 输出：'John Smith'
m.group(1)  # 输出：'John'
m.group(2)  # 输出：'Smith'
```

你可能会想到「[入门篇](http://mp.weixin.qq.com/s?__biz=MzIzMDY0NDQ1Ng==&mid=2247484919&idx=1&sn=7309a9bf1be78ea3250838724ebaa81c&chksm=e8b10a70dfc683666f6d87e6fda6f51863dbaf565ac522b6d01d80059c01a821af70bc279438&scene=21#wechat_redirect)」讲过的**反向引用**，在python中我们依然可以使用序号`\1`、`\2`来引用捕获的组别。下面这个例子将“数字-字母”组成的产品ID进行替换，变成了“字母-数字”的形式。

```python
pattern = re.compile(r"(\d+)-(\w+)")
pattern.sub(r"\2-\1", "1-a\n20-baer\n34-afcr")
# 输出：'a-1\nbaer-20\nafcr-34'
```

我们也可以给每个“笼子”取上名字，即**命名捕获**，在python正则表达式中表示为`(?P<name>pattern)`，依然是通过`group()`访问单个“笼子”。

```python
pattern = re.compile(r"(?P<first>\w+) (?P<last>\w+)")
match = pattern.search("John Smith")
match.group("first") # 输出：'John'
match.group("last") # 输出：'Smith'
```

需要注意，在`sub()`替换操作中，如果用名称来引用组别，我们需要写成`\g<name>`：

```python
pattern = re.compile(r"(?P<country>\d+)-(?P<id>\w+)")
pattern.sub(r"\g<id>-\g<country>", "1-a\n20-baer\n34-afcr")
# 输出：'a-1\nbaer-20\nafcr-34'
```

当然，很多时候我们使用括号不是为了捕获，而仅仅是为了**分组**，即用于构建子表达式、多选结构或者量词作用的对象。此时，我们可以使用**非捕获型括号**`(?:)`，告诉正则引擎，不需要提取括号内的任何信息。非捕获型括号能够提高效率、节约内存，不浪费咱们的“笼子”。

```python
re.search(r"gr(a|e)y", "gray").groups()
# 输出：('a',)
re.search(r"gr(?:a|e)y", "gray").groups()
# 输出：()
```

# 四种类型的环视

**环视**（Look Around）是正则表达式最强大的技术之一。我们可以把环视想象成前视镜和后视镜，**顺序环视**就是向前看（从左到右），**逆序环视**就是向后看（从右到左）。通过左右环视，我们进行“闰土捕鸟”的位置会更精确，确保匹配内容的上下文满足特定要求。

以下是四种类型的环视：

| 类型         | 正则表达式 | 匹配成功的条件           |
| ------------ | ---------- | ------------------------ |
| 肯定顺序环视 | (?=……)     | 子表达式能够匹配右侧文本 |
| 否定顺序环视 | (?!……)     | 子表达式不能匹配右侧文本 |
| 肯定逆序环视 | (?<=……)    | 子表达式能够匹配左侧文本 |
| 否定逆序环视 | (?<!……)    | 子表达式不能匹配左侧文本 |

我们要注意，环视相当于作用于匹配位置的附加条件、不占用任何字符。因此它和分界符类似，是一种**零宽度断言**（zero-width assertions）。我们拿肯定顺序环视举例：

```python
pattern = re.compile(r'(?=fox)')
result = pattern.search("The quick brown fox jumps over the lazy dog")
result.span()
#输出 (16, 16)
```

我们发现，表达式`(?=fox)`只匹配fox之前的位置，也就是索引16。

<img src="C:\Users\13607\AppData\Roaming\Typora\typora-user-images\image-20210218222948086.png" alt="image-20210218222948086" style="zoom: 33%;" />

环视的作用不可小觑，我们可以精准定位，并剔除多余字符，保留更加干净、准确的匹配文本。下面的例子定位`<p>`标签的同时，只匹配标签里的内容。

```python
pattern = re.compile(r'(?<=<p>)[^<]+(?=<p>)')
pattern.search("<p>test<p>").group(0)
# 输出：'test'
```

环视的另一典型应用就是将文本变成逗号分隔的货币形式：

```python
pattern = re.compile(r'\d{1,3}(?=(\d{3})+(?!\d))')
pattern.sub(r'\g<0>,', "123456789")
# 输出：'123,456,789'
```

# 贪婪与非贪婪

在「[入门篇](http://mp.weixin.qq.com/s?__biz=MzIzMDY0NDQ1Ng==&mid=2247484919&idx=1&sn=7309a9bf1be78ea3250838724ebaa81c&chksm=e8b10a70dfc683666f6d87e6fda6f51863dbaf565ac522b6d01d80059c01a821af70bc279438&scene=21#wechat_redirect)」中，我们接触了量词，但并未讲到贪婪与非贪婪的区别。

在python的re模块中，量词默认为**贪婪模式**：尽可能多地匹配更长的字符串。这就是为什么，`.*`通常会匹配到一行文本的末尾。如果要采用**非贪婪模式**，我们可以在量词后添加一个额外的问号，例如`??`、`*?`或`+? `，使得匹配的长度最小。这两种模式又称为**匹配优先**和**忽略优先**。

- 贪婪量词（Greedy quantifiers）：`?`, ` *`,  `+`,  `{num,num}`

- 非贪婪/懒惰量词（Lazy quantifiers）：`??`, `*?`, `+?` ,  `{num,num}?`

比如，如果要匹配引号内的内容，会得到下面的结果：

```python
s = 'The name "McDonald\'s" ! is said "makudonarudo"in Japanese.'
re.search(r'".*"', s).group()
# 输出：'"McDonald\'s" is said "makudonarudo"'
re.search(r'".*?"', s).group()
# 输出：'"McDonald\'s"'
```

事实上，还有第三种量词，但目前python并不支持：

- 占有量词（Possessive quantifiers）：`?+`, `*+`, `++`,  `{num,num}+`

占有量词与贪婪量词类似，只是它们从不会交还已经匹配的字符。当然，我们也可以用固化分组`(?>...)`来代替，比如`!.++`其实等同于`(?>!.+)`。然而，固化分组在python中同样不被支持，所以此处不过多讨论。

以上です！

整理完python正则表达式的进阶知识，相信你已经能得心应手解决很多问题。但是，要真正打造高效、规范、美妙的正则表达式，我们仍需要了解正则引擎的原理，以及一些平衡法则、以及测试和优化的技巧，我们下篇再谈！
