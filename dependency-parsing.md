---
title: 英文依存句法分析
categories:
- 自然语言处理
- 依存分析
tags:
- dependency
- nlp
---
>依存分析是根据句子中单词之间的依存关系来分析句子语法结构的过程。

在依存分析中，各种标签代表了一个句子中两两词语之间的关系。例如，在'rainy weather'这个短语中，rainy是修饰名词weather的形容词。weather -> rainy 形成了依存关系，其中weather是head（中心词），而rainy则是dependent（依赖）。该依存关系用amod标签表示，即形容词修饰语。我们可以用依存关系箭头标注语法关系：
```
     root
      |
      | +-------dobj---------+
      | |                    |
nsubj | |   +------det-----+ | +-----nmod------+
+--+  | |   |              | | |               |
|  |  | |   |      +-nmod-+| | |      +-case-+ |
+  |  + |   +      +      || + |      +      | |
I  prefer  the  morning   flight  through  Denver
```

也可以表示为依存句法树（Dependency Tree Graph）：

![](https://i.loli.net/2021/03/16/JtlAKsfoXhZwLjP.png)

图片来源：[CS224N笔记(五):Dependency Parsing](https://zhuanlan.zhihu.com/p/66268929)

# 通用依存关系

截至目前，UD项目的通用依存关系共有37个，这些关系的完整[列表](https://universaldependencies.org/u/dep/)可以在这里查看并深入研究。

|                                                              | **Nominals**                                                 | **Clauses**                                                  | **Modifier words**                                           | **Function Words**                                           |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Core arguments**                                           | [nsubj](https://universaldependencies.org/u/dep/nsubj.html) [obj](https://universaldependencies.org/u/dep/obj.html) [iobj](https://universaldependencies.org/u/dep/iobj.html) | [csubj](https://universaldependencies.org/u/dep/csubj.html) [ccomp](https://universaldependencies.org/u/dep/ccomp.html) [xcomp](https://universaldependencies.org/u/dep/xcomp.html) |                                                              |                                                              |
| **Non-core dependents**                                      | [obl](https://universaldependencies.org/u/dep/obl.html) [vocative](https://universaldependencies.org/u/dep/vocative.html) [expl](https://universaldependencies.org/u/dep/expl.html) [dislocated](https://universaldependencies.org/u/dep/dislocated.html) | [advcl](https://universaldependencies.org/u/dep/advcl.html)  | [advmod](https://universaldependencies.org/u/dep/advmod.html)* [discourse](https://universaldependencies.org/u/dep/discourse.html) | [aux](https://universaldependencies.org/u/dep/aux_.html) [cop](https://universaldependencies.org/u/dep/cop.html) [mark](https://universaldependencies.org/u/dep/mark.html) |
| **Nominal dependents**                                       | [nmod](https://universaldependencies.org/u/dep/nmod.html) [appos](https://universaldependencies.org/u/dep/appos.html) [nummod](https://universaldependencies.org/u/dep/nummod.html) | [acl](https://universaldependencies.org/u/dep/acl.html)      | [amod](https://universaldependencies.org/u/dep/amod.html)    | [det](https://universaldependencies.org/u/dep/det.html) [clf](https://universaldependencies.org/u/dep/clf.html) [case](https://universaldependencies.org/u/dep/case.html) |
| **Coordination**                                             | **MWE**                                                      | **Loose**                                                    | **Special**                                                  | **Other**                                                    |
| [conj](https://universaldependencies.org/u/dep/conj.html) [cc](https://universaldependencies.org/u/dep/cc.html) | [fixed](https://universaldependencies.org/u/dep/fixed.html) [flat](https://universaldependencies.org/u/dep/flat.html) [compound](https://universaldependencies.org/u/dep/compound.html) | [list](https://universaldependencies.org/u/dep/list.html) [parataxis](https://universaldependencies.org/u/dep/parataxis.html) | [orphan](https://universaldependencies.org/u/dep/orphan.html) [goeswith](https://universaldependencies.org/u/dep/goeswith.html) [reparandum](https://universaldependencies.org/u/dep/reparandum.html) | [punct](https://universaldependencies.org/u/dep/punct.html) [root](https://universaldependencies.org/u/dep/root.html) [dep](https://universaldependencies.org/u/dep/dep.html) |

此外还有一些基于特定语言的依存关系。斯坦福依存分析定义了接近50个依存关系，具体的定义可参考：[Stanford typed dependencies manual](https://nlp.stanford.edu/software/dependencies_manual.pdf)。

关于依存句法分析，也可以参考Daniel Jurafsky的经典NLP书籍Speech and Language Processing相关[章节](https://web.stanford.edu/~jurafsky/slp3/14.pdf)。

<!-- more -->

# 工具推荐

## Stanford Parser句法分析

比较有名的工具是Stanford Parser，我们可以在[这里](http://nlp.stanford.edu:8080/corenlp/)在线使用并进行可视化。

或者安装斯坦福的Python NLP软件包[Stanza](https://stanfordnlp.github.io/stanza/installation_usage.html)，里面集成了[依存关系分析](https://stanfordnlp.github.io/stanza/depparse.html)工具。

如何解决download()下载异常的问题可以参考[这里](https://blog.csdn.net/superyangtze/article/details/105252193)。

我们可以进行词性标注：

```python
import stanza
# stanza.download('en')
nlp = stanza.Pipeline('en')
doc = nlp('It took me more than two hours to translate a few pages of English.')
for sentence in doc.sentences:
    for word in sentence.words:
        print(word.text, word.lemma, word.pos)
'''
It it PRON
took take VERB
me I PRON
more more ADJ
than than ADP
two two NUM
hours hour NOUN
to to PART
translate translate VERB
a a DET
few few ADJ
pages page NOUN
of of ADP
English English PROPN
. . PUNCT
'''
```
以及依存句法分析：
```python
import stanza
# stanza.download('en')
nlp = stanza.Pipeline('en')
doc = nlp('It took me more than two hours to translate a few pages of English.')
print(*[f'id: {word.id}\tword: {word.text}\thead id: {word.head}\thead: {sent.words[word.head-1].text if word.head > 0 else "root"}\tdeprel: {word.deprel}' for sent in doc.sentences for word in sent.words], sep='\n')
# 输出：
id: 1	word: It	head id: 2	head: took	deprel: expl
id: 2	word: took	head id: 0	head: root	deprel: root
id: 3	word: me	head id: 2	head: took	deprel: iobj
id: 4	word: more	head id: 6	head: two	deprel: advmod
id: 5	word: than	head id: 4	head: more	deprel: fixed
id: 6	word: two	head id: 7	head: hours	deprel: nummod
id: 7	word: hours	head id: 2	head: took	deprel: obj
id: 8	word: to	head id: 9	head: translate	deprel: mark
id: 9	word: translate	head id: 2	head: took	deprel: csubj
id: 10	word: a	head id: 12	head: pages	deprel: det
id: 11	word: few	head id: 12	head: pages	deprel: amod
id: 12	word: pages	head id: 9	head: translate	deprel: obj
id: 13	word: of	head id: 14	head: English	deprel: case
id: 14	word: English	head id: 12	head: pages	deprel: nmod
id: 15	word: .	head id: 2	head: took	deprel: punct
```
## Spacy依存句法分析

我们也可以使用Spacy进行依存句法分析并画出句法树。

```python
# $python -m spacy download en_core_web_sm
import spacy
nlp=spacy.load('en_core_web_sm')
text='It took me more than two hours to translate a few pages of English.'
for token in nlp(text):
	print(token.text,'=>',token.dep_,'=>',token.head.text)
```

输出结果如下，对比stanza的结果，还是有明显差异，例如此处it被标记为`nsubj`，但其实这种情况下it是虚词expletive（我们都学过形式主语），不担任谓词的语义角色，因此应该标记为expl。

```
It => nsubj => took
took => ROOT => took
me => dative => took
more => amod => two
than => quantmod => two
two => nummod => hours
hours => dobj => took
to => aux => translate
translate => xcomp => took
a => det => pages
few => amod => pages
pages => dobj => translate
of => prep => pages
English => pobj => of
. => punct => took
```

画图的方法参考[spacy可视化工具](https://spacy.io/usage/visualizers)，如果是在sublime text之类的编辑器内运行代码，我们需要打开浏览器地址http://localhost:5000/查看画图的结果。如果是jupyter notebook，则可以直接显示图像。

```python
import spacy
from spacy import displacy

nlp = spacy.load("en_core_web_sm")
doc = nlp("It took me more than two hours to translate a few pages of English.")
displacy.serve(doc, style="dep")
```

![](https://i.loli.net/2021/03/16/dInSpW2aBw1Dg4u.png)

spacy中的依存关系有45个，完整的标签如下，具体的定义可以参考：[Stanford typed dependencies manual](https://nlp.stanford.edu/software/dependencies_manual.pdf)。


| 标签        | 定义                                         |
| ----------- | -------------------------------------------- |
| `ROOT`      | root                                         |
| `acl`       | clausal modifier of noun (adjectival clause) |
| `acomp`     | adjectival complement                        |
| `advcl`     | adverbial clause modifier                    |
| `advmod`    | adverbial modifier                           |
| `agent`     | agent                                        |
| `amod`      | adjectival modifier                          |
| `appos`     | appositional modifier                        |
| `attr`      | attribute                                    |
| `aux`       | auxiliary                                    |
| `auxpass`   | auxiliary (passive)                          |
| `case`      | case marking                                 |
| `cc`        | coordinating conjunction                     |
| `ccomp`     | clausal complement                           |
| `compound`  | compound                                     |
| `conj`      | conjunct                                     |
| `csubj`     | clausal subject                              |
| `csubjpass` | clausal subject (passive)                    |
| `dative`    | dative                                       |
| `dep`       | unclassified dependent                       |
| `det`       | determiner                                   |
| `dobj`      | direct object                                |
| `expl`      | expletive                                    |
| `intj`      | interjection                                 |
| `mark`      | marker                                       |
| `meta`      | meta modifier                                |
| `neg`       | negation modifier                            |
| `nmod`      | modifier of nominal                          |
| `npadvmod`  | noun phrase as adverbial modifier            |
| `nsubj`     | nominal subject                              |
| `nsubjpass` | nominal subject (passive)                    |
| `nummod`    | numeric modifier                             |
| `oprd`      | object predicate                             |
| `parataxis` | parataxis                                    |
| `pcomp`     | complement of preposition                    |
| `pobj`      | object of preposition                        |
| `poss`      | possession modifier                          |
| `preconj`   | pre-correlative conjunction                  |
| `predet`    | predeterminer                                |
| `prep`      | prepositional modifier                       |
| `prt`       | particle                                     |
| `punct`     | punctuation                                  |
| `quantmod`  | modifier of quantifier                       |
| `relcl`     | relative clause modifier                     |
| `xcomp`     | open clausal complement                      |
