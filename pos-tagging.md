---
title: "英文词性标记（POS Tagging）"
categories:
- 自然语言处理
- 词性标注
tags:
- pos
- nlp
---
# 词性标记（POS Tagging）

POS标签大致分为两种：通用POS标签和细粒度POS标签。

## 跨语言的通用POS标签

通用依存关系（[UD](https://universaldependencies.org/)）是一个致力于开发跨语言树库标注的项目，为不同语言标注一致的语法[^1]。
<!-- more -->
该标注方案基于（通用的）斯坦福依存关系（de Marneffe et al., 2006, 2008, 2014）、Google通用词性标签（Petrov et al., 2012）和Interset interlingua的形态语义标签集（Zeman, 2008）的发展进化。

该[网站](https://universaldependencies.org/u/pos/index.html)列举了各个通用POS标签的详细解释。

[^1]: [词性标记和依存关系分析](https://www.analyticsvidhya.com/blog/2020/07/part-of-speechpos-tagging-dependency-parsing-and-constituency-parsing-in-nlp/#:~:text=Dependency%20parsing%20is%20the%20process,tags%20are%20the%20dependency%20tags.)

| Tag                                                         | Category                  | Explanation                                                  | Examples                         |
| ----------------------------------------------------------- | ------------------------- | ------------------------------------------------------------ | -------------------------------- |
| [ADJ](https://universaldependencies.org/u/pos/ADJ.html)     | adjective                 | 形容词，修饰名词或充当谓语                                   | big, African, first              |
| [ADP](https://universaldependencies.org/u/pos/ADP.html)     | adposition                | 介词，包括前置词和后置词（prepositions and postpositions）   | in, during                       |
| [ADV](https://universaldependencies.org/u/pos/ADV.html)     | adverb                    | 副词，修饰动词、形容词和副词本身，表示时间、地点、方向或方式 | very, up, tomorrow, where, never |
| [AUX](https://universaldependencies.org/u/pos/AUX_.html)    | auxiliary                 | 助词，包括Tense auxiliaries, Passive auxiliaries, Modal auxiliaries, Verbal copulas | has, was, should                 |
| [CCONJ](https://universaldependencies.org/u/pos/CCONJ.html) | coordinating conjunction  | 并列连词                                                     | and, or, but                     |
| [DET](https://universaldependencies.org/u/pos/DET.html)     | determiner                | 限定词，修饰名词                                             | the, a, an                       |
| [INTJ](https://universaldependencies.org/u/pos/INTJ.html)   | interjection              | 感叹词                                                       | ouch, yes                        |
| [NOUN](https://universaldependencies.org/u/pos/NOUN.html)   | noun                      | 普通名词                                                     | girl, tree, air                  |
| [NUM](https://universaldependencies.org/u/pos/NUM.html)     | numeral                   | 数词                                                         | 2014, one, II                    |
| [PART](https://universaldependencies.org/u/pos/PART.html)   | particle                  | 小品词，与其他词语结合                                       | 's, not                          |
| [PRON](https://universaldependencies.org/u/pos/PRON.html)   | pronoun                   | 代词，代替名词或名词短语                                     | you, who, somebody, it           |
| [PROPN](https://universaldependencies.org/u/pos/PROPN.html) | proper noun               | 专有名词，特定任务、地点或物体的名称                         | Mary, London, NATO               |
| [PUNCT](https://universaldependencies.org/u/pos/PUNCT.html) | punctuation               | 标点符号是非字母字符和字符组                                 | . , ()                           |
| [SCONJ](https://universaldependencies.org/u/pos/SCONJ.html) | subordinating conjunction | 从属连词，引入从句的连词                                     | since, that, who, if, while      |
| [SYM](https://universaldependencies.org/u/pos/SYM.html)     | symbol                    | 符号，可以用普通单词代替（比如$换作美元）                    | $, %, ♥‿♥, 😝                     |
| [VERB](https://universaldependencies.org/u/pos/VERB.html)   | verb                      | 动词，表示事件和动作                                         | run, runs, running               |
| [X](https://universaldependencies.org/u/pos/X.html)         | other                     | 其他                                                         | xfgh                             |

该项目的[Github地址](https://github.com/UniversalDependencies/docs)涵盖了不同语言。

## 特定语言的细粒度POS标签

我们可以将通用POS标签细化，例如将英语中的NOUN（普通名词）进一步划分为复数普通名词（NNS），NN（单数普通名词），但这些标签是基于特定语言的。

我们可以打开[spacy](https://spacy.io/usage/linguistic-features)，线上运行以下代码。其中pos表示[通用POS标签集](https://universaldependencies.org/docs/u/pos/)的粗粒度[词性](https://universaldependencies.org/docs/u/pos/)，而tag表示细粒度的词性。

```python
import spacy

nlp = spacy.load("en_core_web_sm")
doc = nlp("It took me more than two hours to translate a few pages of English.")
for token in doc:
	print(token.text, '=>',token.pos_,'=>',token.tag_)
```

运行结果如下：

```
It => PRON => PRP
took => VERB => VBD
me => PRON => PRP
more => ADJ => JJR
than => SCONJ => IN
two => NUM => CD
hours => NOUN => NNS
to => PART => TO
translate => VERB => VB
a => DET => DT
few => ADJ => JJ
pages => NOUN => NNS
of => ADP => IN
English => PROPN => NNP
. => PUNCT => .
```

有关spaCy模型在不同语言中分配的细粒度和粗粒度词性标签的列表，请参阅[models目录中](https://spacy.io/models)记录的标签方案。

比如英语的所有细粒度[标签](https://spacy.io/models/en)如下：

| A       | B                                         |
| ------- | ----------------------------------------- |
| `$`     | symbol, currency                          |
| `''`    | closing quotation mark                    |
| `,`     | punctuation mark, comma                   |
| ````    | opening quotation mark                    |
| `-LRB-` | left round bracket                        |
| `-RRB-` | right round bracket                       |
| `.`     | punctuation mark, sentence closer         |
| `:`     | punctuation mark, colon or ellipsis       |
| `ADD`   | email                                     |
| `AFX`   | affix                                     |
| `CC`    | conjunction, coordinating                 |
| `CD`    | cardinal number                           |
| `DT`    | determiner                                |
| `EX`    | existential there                         |
| `FW`    | foreign word                              |
| `HYPH`  | punctuation mark, hyphen                  |
| `IN`    | conjunction, subordinating or preposition |
| `JJ`    | adjective                                 |
| `JJR`   | adjective, comparative                    |
| `JJS`   | adjective, superlative                    |
| `LS`    | list item marker                          |
| `MD`    | verb, modal auxiliary                     |
| `NFP`   | superfluous punctuation                   |
| `NN`    | noun, singular or mass                    |
| `NNP`   | noun, proper singular                     |
| `NNPS`  | noun, proper plural                       |
| `NNS`   | noun, plural                              |
| `PDT`   | predeterminer                             |
| `POS`   | possessive ending                         |
| `PRP`   | pronoun, personal                         |
| `PRP$`  | pronoun, possessive                       |
| `RB`    | adverb                                    |
| `RBR`   | adverb, comparative                       |
| `RBS`   | adverb, superlative                       |
| `RP`    | adverb, particle                          |
| `SYM`   | symbol                                    |
| `TO`    | infinitival "to                           |
| `UH`    | interjection                              |
| `VB`    | verb, base form                           |
| `VBD`   | verb, past tense                          |
| `VBG`   | verb, gerund or present participle        |
| `VBN`   | verb, past participle                     |
| `VBP`   | verb, non-3rd person singular present     |
| `VBZ`   | verb, 3rd person singular present         |
| `WDT`   | wh-determiner                             |
| `WP`    | wh-pronoun, personal                      |
| `WP$`   | wh-pronoun, possessive                    |
| `WRB`   | wh-adverb                                 |
| `XX`    | unknown                                   |
