---
title: 《自然语言处理综论》第14章-依存分析（上）
categories:
- 自然语言处理
- 依存分析
tags:
- nlp
- dependency
mathjax: true
---
<center>
<i>原文链接：https://web.stanford.edu/~jurafsky/slp3/14.pdf</i>
<br>
<i>译者：鸽鸽（自己学习使用，非商业用途）</i>
</center>

----

前两章的重点是**上下文无关语法**（context-free grammars）[^1]，及其用于自动生成基于成分的表征。 我们在这里介绍另一种称为**依存语法**（dependency grammars）的语法形式主义派系，它在当代语音和语言处理系统中极其重要。 在这些形式主义中，短语成分（phrasal constituents）和短语结构规则（phrase-structure rules）并不直接发挥作用。相反，一个句子的句法结构，仅根据句中单词（或词元lemmas）以及单词间存在的一组关联的有向二元语法关系来描述。

[^1]: 也称为短语结构语法 (phrase-structure grammar)

下图展示了标准的依存分析风格的句法图示。
<center><img src="https://i.loli.net/2021/03/17/3XaE6umlQIgpZrs.png"  alt="" width="700" /></center>

在句子上方，用从**头部**（heads）到**依存项**（dependents）的有向的标记弧来表示单词之间的关系。我们称之为**类型依存结构**（typed dependency structure），因为标签是从固定的语法关系清单中提取的。它还包括一个根（root）节点，显式地标记句法树的根，即整个结构的中心。

<!-- more -->

图14.1显示了与第12章中给出的相应短语结构分析相同的依存分析及树形结构。注意依存分析中没有对应短语成分或词汇类别的节点；**其内部结构仅由句子中词汇项之间的定向关系组成。**这些关系**直接编码重要信息**，这些信息往往隐藏在更复杂的短语结构分析中。例如，动词prefer的**论元**（arguments）在依存结构中直接链接到它，而在短语结构树中它们与主动词的连接不那么紧密。类似地，flight的修饰语morning和Denver在依存结构中直接与之链接。

<center><img src="https://i.loli.net/2021/03/17/c1yRPAQCefvESZW.png"  alt="" width="700" /></center>

**依存语法的一个主要优势是能够处理形态丰富、词序相对自由（free word order）的语言。**例如，捷克语的词序可能比英语灵活得多；宾语可能出现在位置状语之前或之后。短语结构语法会需要为解析树中每个可能出现这样一个状语短语的位置单独制定一条规则。基于依存关系的方法只用一种连接类型来表示这种特殊的状语关系。因此，依存分析的方法抽象出了词序信息，只表示解析所需的信息。

使用依存分析的另一个实际性的动机是，**头部-依存（head-dependent）关系**提供了一种近似于谓词及其论元之间的语义关系，这使得它们对指代消歧、自动问答和信息提取之类的许多应用都能产生直接的帮助。基于成分（constituent-based）的语法解析也提供了类似的信息，但通常必须通过诸如第12章讨论的中心语规则等技术从树中提炼出来。

在下面的章节，我们将更详细地讨论依存分析中使用的关系清单，以及这些依存结构的形式基础。然后我们将继续讨论用于自动生成这些结构的主流算法派系。最后，我们将讨论如何评估依存分析器，并指出它们在语言处理中应用的一些方式。

# 14.1 依存关系

传统语言学的语法关系概念为构成这些依存结构的二元语法关系提供了基础。这些头关系（head relations）的参数由一个**头部**（heads）和一个**依存项**（dependents）组成。在第12章和附录C中，我们已经在成分结构的语境下中讨论过头部的依存项这个概念。在那里，一个成分的头部是一个更大成分的中心组织词（例如名词短语中的关键名词，或动词短语中的动词）。成分中其余的词都是其头部的直接或间接的依存项。在基于依存关系的方法中，通过直接将头部与紧靠头部的词连接起来，绕过成分结构，使头部-依存关系变得明确。

除了指定头部-依存对，依存语法还允许我们根据依存项相对于头部的作用，进一步划分语法关系种类或**语法功能**（grammatical function）。我们熟悉的主语、直接宾语和间接宾语等概念都是可能会想到的关系种类。在英语中，这些概念虽然与一个词在句中的位置和成分类型密切相关，但不起决定性作用，因此与短语结构树中提供的信息重复累赘。然而，在更灵活的语言中，直接编码这些语法关系中的信息是至关重要的，因为基于短语的成分句法提供的帮助很小。

毫不奇怪，语言学家们已经发明了远远超出我们熟悉的主语和宾语概念的关系分类学。虽然不同的理论之间大相径庭，但有足够的共性使其发展出一个在计算上有用的标准。**通用依存关系**（Universal Dependencies）项目（Nivre et al.，2016）提供了一个语言驱动的、利于计算的、跨语言适用的依存关系清单。

| Clausal Argument Relations     | Description                                        |
| ------------------------------ | -------------------------------------------------- |
| NSUBJ                          | Nominal subject                                    |
| DOBJ                           | Direct object                                      |
| IOBJ                           | Indirect object                                    |
| CCOMP                          | Clausal complement                                 |
| XCOMP                          | Open clausal complement                            |
| **Nominal Modifier Relations** | **Description**                                    |
| NMOD                           | Nominal modifier                                   |
| AMOD                           | Adjectival modifier                                |
| NUMMOD                         | Numeric modifier                                   |
| APPOS                          | Appositional modifier                              |
| DET                            | Determiner                                         |
| CASE                           | Prepositions, postpositions and other case markers |
| **Other Notable Relations**    | **Description**                                    |
| CONJ                           | Conjunct                                           |
| CC                             | Coordinating conjunction                           |

<center><i>图14.2 通用依存关系集中的部分依存关系  (de Marneffe et al., 2014)</i></center>

图14.2显示了这项工作中的关系子集。图 14.3 提供了一些例句来说明选定的关系。通用依存方案中所有关系的来由超出了本章的范围，但常用关系的核心集可以分成两组：描述与谓语（通常是动词）有关的句法角色的子句关系（clausal relations），以及对头部修饰词进行分类的修饰关系（modifier relations）。

| Relation | Examples with *head* and dependent                    |
| -------- | ----------------------------------------------------- |
| NSUBJ    | **United** *canceled* the flight.                     |
| DOBJ     | United *diverted* the **flight** to Reno.             |
|          | We *booked* her the first **flight** to Miami.        |
| IOBJ     | We *booked* **her** the flight to Miami.              |
| NMOD     | We took the **morning** *flight*.                     |
| AMOD     | Book the **cheapest** *flight*.                       |
| NUMMOD   | Before the storm JetBlue canceled **1000** *flights*. |
| APPOS    | *United*, a **unit** of UAL, matched the fares.       |
| DET      | **The** *flight* was canceled.                        |
|          | **Which** *flight* was delayed?                       |
| CONJ     | We *flew* to Denver and **drove** to Steamboat.       |
| CC       | We flew to Denver **and** *drove* to Steamboat.       |
| CASE     | Book the flight **through** *Houston*.                |

Figure 14.3 Examples of core Universal Dependency relations.

参考以下例句，子句关系NSUBJ和DOBJ分别表示主语和谓语cancel的直接宾语，而NMOD、DET和CASE关系表示名词flights和Houston的修饰语。

![](https://i.loli.net/2021/03/17/XgwzFkLVSdaIfDj.png)

# 14.2 依存形式主义

在最普通的形式中，我们讨论的依存关系结构仅仅是有向图，即由一组顶点$V$和一组有序的顶点对$A$组成的结构$G=(V, A)$，我们称之为弧（arcs）。

大多数情况下，我们假设顶点集$V$完全对应于给定句子中单词的集合。然而，它们也可能对应于标点符号，或者当处理形态复杂的语言时，顶点集可能由词干和词缀组成。弧线集$A$捕获了$V$中元素之间的头部-依存关系和语法功能关系。

对这些依存结构的进一步限制是针对底层语法理论或形式主义的。其中比较常见的限制是，这些结构必须是连接的、有一个指定的根节点，并且是无环或平面的。与本章讨论的解析方法最相关的是对有根树的常见的、以计算为目的的限制。也就是说，**依存树**（dependency tree）是一个满足以下约束的有向图。

1. 有一个指定的根结点，它没有传入弧。
2. 除根节点外，每个顶点恰好有一个传出弧。
3. 从根节点到$V$中的每个顶点有一条唯一的路径。

综上所述，这些约束条件保证了每个词都有一个头，依存结构是连接的，并且有唯一的根节点，从这个根节点可以沿着唯一的定向路径到句子中的每个词。

## 14.2.1 投射性

**投射性**（projectivity）的概念施加了一个额外的约束条件，这个约束条件来自于输入（input）中词的顺序。如果在句子中**存在一条从头部到位于头部和依存项之间的每个词的路径**，那么就说这条从头部到依存项的弧线具有投射性。如果组成依存树的所有弧线都有投射性，那么就可以说它是投射的。到目前为止，我们看到的所有依存树都是投射的。然而，有许多完全合乎规则的结构会生成非投射树，特别是在词序相对灵活的语言中。

请看下面的例子。

![](https://i.loli.net/2021/03/17/2Jicfx1AThkBFId.png)

在这个例子中，从flight到它的修饰词was的弧线是非投射的，因为从flight到中间的单词this和morning没有路径。正如我们从这张图中看到的，投射性（和非投射性）可以通过画树的方式来检测。**如果能画出没有交叉边的依存树，那么它就是投射性的。**在这里，如果不跨越连接morning和它的头部的弧线，就无法将flight和它的依存项was联系起来。

我们对投射性的关注来自于两个相关的问题。首先，最广泛使用的英语依存关系树库是通过使用中心语查找规则从短语结构树库中自动导出的（第12章）。以这种方式生成的树是保证投射性的，因为它们是由上下文无关语法生成的。第二，最广泛使用的一系列解析算法存在计算上的限制。第14.4节中讨论的基于转换的方法只能生成投射树，因此任何具有非投射结构的句子都必然会出错。这个限制是第14.5节中描述的更灵活的基于图的解析方法的动机之一。

# 14.3 依存树库

与基于成分的方法一样，树库（treebanks）在依存分析器（dependency parsers）的开发和评估中起着至关重要的作用。**依存树库**（dependency treebanks）的创建方法与第12章中讨论的方法类似——让人类标注者直接为给定的语料库生成依存结构，或者使用自动解析器（automatic parsers ）提供初始解析，然后让标注者手动修正这些解析器。我们也可以用一个确定性过程[^2]（deterministic process）通过标注中心语规则将现有的基于成分的树库翻译成依存树。

[^2]: 译者注：对应[随机过程（*Stochastic Process*）](https://baike.baidu.com/item/随机过程)

大多形态丰富的语言（如捷克语、印地语和芬兰语）都已经建立了直接标注的依存树库用于依存分析，其中捷克语的Prague依存树库（Bejcek et al., 2013）是最著名的工作。主流英语依存树库主要是从现有资源中提取出来的，比如Penn树库的华尔街日报部分（Marcus等人，1993）。最近的OntoNotes项目（Hovy et al. 2006, Weischedel et al. 2011）扩展了这种方法，超越了传统的新闻文本，涵盖了英语、汉语和阿拉伯语的电话对话、网络日志、usenet新闻组、广播和脱口秀。

从成分结构到依存结构的翻译过程有两个子任务：识别结构中所有的头部-依存关系，以及正确识别这些关系的种类。第一个任务主要依赖于第12章中讨论的中心语规则（head rules）的使用，这些规则最早是为词汇化概率解析器（ lexicalized probabilistic parsers）而开发的(Magerman 1994, Collins 1999, Collins 2003)。下面是Xia和Palmer（2001）提出的一个简单有效的算法。

1. 使用适当的中心语规则，标记短语结构中每个节点的头部子节点。
2. 在依存结构中，让每个非头部子节点的头部依存于头部子节点的头部。

当一个短语结构解析包含了额外的语法关系和函数标签形式的信息时，如在Penn Treebank的情况下，这些标签可以用来标记生成的树的边。当应用于图14.4中的解析树时，这种算法将产生例14.4中的依存结构。这些提取方法的主要缺点是它们受到原始结构树中存在的信息的限制。其中最重要的问题是未能将形态学信息与短语结构树整合在一起，不能轻易地表示非宾语结构，以及大多数名词短语缺乏内部结构，这反映在大多数树库语法通常所使用的平面规则中。由于这些原因，除了英语之外，大多数依存树库都是直接靠人类标注者开发的。

<u>本章剩余内容见：[《自然语言处理综论》第14章-依存分析（中）](http://nlpcourse.cn/2021/03/17/dependency-parsing-3/)</u>


