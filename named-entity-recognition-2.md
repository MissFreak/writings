---
title: 英文文献的命名实体识别
categories:
- 自然语言处理
- 命名实体识别
tags:
- nlp
- NER
mathjax: true
---
命名实体识别（NER）是指自动识别文本中的命名实体并将其分类为预定义的类别。实体可以是人员、组织、位置、时间、数量、货币价值、百分比等的名称。作为信息抽取的基础步骤，NER从非结构化文本中提取关键元素，因此它是一项至关重要的技术。

我们可以创建自己的实体类别以适应不同的任务。现在有许多出色的开源库，包括[NLTK](https://www.nltk.org/)，[SpaCy](https://spacy.io/)和[Stanford NER](https://nlp.stanford.edu/software/CRF-NER.shtml)。如何用这些工具实现，参考[命名实体识别：NLP从业人员指南](https://www.kdnuggets.com/2018/08/named-entity-recognition-practitioners-guide-nlp-4.html)和[NLTK和SpaCy命名实体识别](https://towardsdatascience.com/named-entity-recognition-with-nltk-and-spacy-8c4a7d88e7da)。也可以参考[这篇](https://monkeylearn.com/blog/named-entity-recognition/)文章。

<!-- more -->

## 学术文献

https://livebook.manning.com/book/natural-language-processing-in-action/chapter-11/

https://github.com/AmoDinho/datacamp-python-data-science-track/blob/master/Natural%20Language%20Processing%20Fundamentals%20in%20Python/Chapter%203%20-Named-entity%20recognition.py

https://www.sciencedirect.com/topics/computer-science/named-entity-recognition

https://www.nltk.org/book/ch07.html

明天要翻译的文献：信息抽取

https://web.stanford.edu/~jurafsky/slp3/17.pdf


# 如何训练NER分类器

通常使用BIO表示法，该表示法区分实体的开始（B）和内部（I），O标记非实体。NER往往需要特定领域的训练，尤其是更细粒度的NER。



## 评估

*F-Score*是用于评估NER的一种常用度量，它是Precision和Recall的组合。Precision, recall, and F-Score are defined as follows ([Campos et al., 2012](https://www.frontiersin.org/articles/10.3389/fcell.2020.00673/full#B21)):

$$
\begin{array}{c}
\text { Precision }=\frac{\text { Relevant Names Recognized }}{\text { Total Names Recognized }} \\
=\frac{\text { True Positives }}{\text { True Positives+False Positives }} \\
\text { Recall }=\frac{\text { Relevant Names Recognized }}{\text { Relevant Names in Corpus }} \\
=\frac{\text { True Positives }}{\text { True Positives+False Negatives }} \\
\text { F-score }=2 \times \frac{\text { Precision } \times \text { Recall }}{\text { Precision+Recáll }}
\end{array}
$$