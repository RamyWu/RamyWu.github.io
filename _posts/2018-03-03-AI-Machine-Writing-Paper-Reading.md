---
layout: post
title:  "AI 自动写作 · 论文阅读"
date:   2018-03-03 20:12:00 +0800
tags: [AI-PM,Paper-Reading]
category: Work
---


上一篇：[AI自动写作·产品调研](http://www.ramywu.com/work/2018/03/02/AI-Machine-Writing-Survey/)，主要了解了当下的文本生成在商业领域的应用和业务场景。本篇是AI自动写作相关论文笔记，主要目的是想了解底层技术逻辑，虽然基本上没看懂，但是也有了一个大体的流程和了解。


## 诗歌生成

### 传统方法的诗歌生成：基于遗传算法的方法


PaperWeekly-解读：

> 传统方法非常依赖于诗词领域的专业知识，需要专家设计大量的人工规则，对生成诗词的格律和质量进行约束。同时迁移能力也比较差，难以直接应用到其他文体（唐诗，宋词等）和语言（英文，日文等）
> 
> 基于遗传算法的方法，将诗歌生成看成状态空间搜索问题。先从随机诗句开始，然后借助人工定义的诗句评估函数，不断进行评估，进化的迭代，最终得到诗歌。这种方法在单句上有较好的结果，但是句子之间缺乏语义连贯性。

周昌乐等构建了一种基于遗传算法的宋词生成计算模型，由生成模块和评价模块两部分组成：


> - 生成模块根据**词法、句法、概念**等信息**产生备选诗作**
> - 评价模块则依据一定的**准则**对备选输出给予**等级评价**
> 

首先遇到的问题：为了使机器能够产生好的宋词，首先要解决宋词句法规律与语义度量的计算问题。

> 要给出衡量宋词优劣与否的量化计算方法，作为适应度函数的构造与计算依据。
> 
> 诗歌的质量主要反映在**句法**和**语义**两个层次上：
> 
> - 句法：既包括通常汉语所需遵循的句法，又包括诗歌特有的格律规则：如平仄、押韵等规则
> - 语义：包括了主题与词句的连贯、风格的统一、情感与意境的传达等等
> 

**句法层次：**

> 宋词作为一种特殊的文体，其句法也有特定的要求。一般每个词牌的词体句法都有固定的总字数、总句数，每一句的字数也是固定的。采用DFA(deterministicfiniteautomata)的句法判定规范。

**语义层次：**

> 最关键的问题是：
> 
> - 如何使产生的诗句看起来更有意义
> - 使句与句之间更有连贯性
> - 而不是毫无关联的词汇或句子的堆砌
> 

宋词的语义计算问题，包括**词义相关度计算**、**词义相似度计算**，以及**风格情感一致性计算**3个方面。

> - 计算词义相关度：目的是建立词语间的关联，发掘词语共现和搭配的可能，从而保证生成诗词行文和主题上的连贯
> - 计算词语相似度：词语相似度主要用于衡量文本中词语的可替换程度。计算词义相似度，目的是在保证所选词紧扣主题的前提下，尽量使生成诗词的语言更丰富多变。
> - 风格情感一致性计算：在全宋词风格与情感标注的基础上，对词语做简单风格与情感分类统计来作为计算依据
> 
> > 将词语集分为柔和、中性、强烈3个子集，然后递归地对各个子集进行相应的操作，最后将词语集分为7个不同意味的子集，用数字分别表示为−3、−2、−1、0、+1、+2、+3这7种水平，分数越高，代表该词语能够体现某种风格的贡献度越强。

~对诗词风格的评价指标和方式，这里又拓展阅读了下另一篇论文[基于词联接的诗词风格评价技术-中国知网](http://kns.cnki.net/KCMS/detail/detail.aspx?dbcode=CJFQ&dbname=CJFD2005&filename=MESS200506014&uid=WEEvREcwSlJHSldTTEYzVDhsOWpuU1lNU3Y4akU4VWtFTTg2SjZRT3pEYz0=$9A4hF_YAuvQ5obgVAqNKPCYcEjKensW4IQMovwHtwkF4VYPoHbKxJw!!&v=MjQ0ODBLQ2pZZmJHNEh0VE1xWTlFWUlSOGVYMUx1eFlTN0RoMVQzcVRyV00xRnJDVVJMS2ZZK1J2RnlIbFc3N0w=)，根据事物形式上的确定其风格的柔和或强烈意味：

> 
> **事物的形式包括数量、形体、色彩、声音、味道、重量、力量、节奏、韵律、速度、质感等许多因素**，能够给人丰富的意味体验。
> 
> - 一般而言，具有数量少、形体小、色彩素、声音柔、味道淡、重量轻、速度慢等形式特征的事物更容易引起人的柔和意味，美学上往往称为“优美”，“指小巧、细腻、柔和的美”；
> - 具有数量多、形体大、色彩艳、声音粗、味道浓、重量沉、速度快等形式特征的事物更容易引起人的强烈意味，美学上往往称为“壮美”，“指巨大、粗犷、豪放的美”。
> 
> 这是人们在艺术实践中摸索出的普遍规律。该规律将人对事物形式的审美体验的质与事物形式的量相联系，从而具有很大的理论与实践意义。**作品宏观上的豪放与婉约风格决定于词汇所指事物微观上形式的**。
> 
> 有两个问题需要解决：
> 
> 1. 确定参照点
> 2. 确定意味水平
> 
> 这两个问题的解决方式：成立专家组，通过讨论或其它互动方式一致将词集分为柔和、中性、强烈三个子集，然后递归地对各个子集进行相应的操作，递归的层次依精确需求而定。
> 
> 《基于词联接的诗词格评价技术》

评测方式：

> 目前对于机器艺术作品质量的评测主要通过图灵测验性方式进行，也采用评判专家组来进行宋词生成结果的评测：
> 
> 针对主题相关度评判、风格情感一致性评判和总体质量评判这3个指标进行评测。评判专家组由5名中文系本科生组成，评判采用5分制。

---

### 基于深度学习技术的诗歌生成

这里直接看[PaperWeekly第二十三期---机器写诗](https://mp.weixin.qq.com/s?__biz=MzIwMTc4ODE0Mw==&mid=2247484024&idx=1&sn=8dd29ba9a3eb22361b350f9be18db012&chksm=96e9ddf8a19e54eeaa8508ca462593668fe63c873de6fbf5d38f8d7ddb49b0787419cc0dd228&scene=21#wechat_redirect)，有几种不同的方法。

---

## 球赛新闻

### 足球赛事战报的自动写作研究

核心思路：


> - 首先，分析处理已有的战报，找到实时数据中编辑们认为最为关键的事件，间接地完成对实时数据进行标注，得到训练集。
> - 然后，训练一个基于卷积神经网络的模型，自动识别实时数据中的关键事件。由于实时数据中定义的事件能够精确地反映事件发生的时间、位置、动作目标，甚至助攻队友。
> - 最后，取出关键事件后，仅需针对不同类型的事件制作少量的模板句，再将这些模板句填入模板库，一篇生动详实的战报便呈现在读者面前。

足球赛事战报流程

> ![](http://ojcp18ifz.bkt.clouddn.com/2018-04-20-105018.jpg)

评价方式：

> 评价一篇体育赛事战报的指标因人而异。从读者角度出发，本文出 3 个方面的评价指标，分别是关键事件覆盖率(critical incident coverage)、细节还原率(detail reduction rate)和语言表达正确率(correct expression rate)。

### NBA赛事新闻的自动写作研究

核心思路：

> 本文通过分析 NBA 文字直播的特点，构建一种 NBA 赛事新闻自动生成方法。首先，根据文字直播的特点构建球队的分差函数，并提出基于分差函数的数据分片算法和数据合成算法，然后对数据片进行分类，构建模板库，从而构建 NBA 赛事新闻自动生成的模型。

评价方式：

> NBA 赛事新闻的自动写作缺乏通用的评价标准，本文将生成时间作为一种评价指标，然后采用人工评价的方法，请 3 名 NBA 球迷进行评价，采用 3 种评价指标：指标1，是否由计算机所写；指标 2，是否符合文字直播的真实情况；指标 3，语言是否生动。

## 参考论文

新闻类（赛报）

- [足球赛事战报的自动写作研究-中国知网](http://kns.cnki.net/KCMS/detail/11.2442.N.20171105.1715.003.html)
- [NBA赛事新闻的自动写作研究-中国知网](http://kns.cnki.net/KCMS/detail/detail.aspx?dbcode=CJFQ&dbname=CJFDLAST2017&filename=BJDZ201702003&uid=WEEvREcwSlJHSldTTEYzVDhsOWpuU1lNU3Y4akU4VWtFTTg2SjZRT3pEYz0=$9A4hF_YAuvQ5obgVAqNKPCYcEjKensW4IQMovwHtwkF4VYPoHbKxJw!!&v=MTIyODdmWStSdkZpam5WYnJJSnlmUGRMRzRIOWJNclk5Rlo0UjhlWDFMdXhZUzdEaDFUM3FUcldNMUZyQ1VSTEs=)


人文类：（诗歌）


- [PaperWeekly第二十三期---机器写诗](https://mp.weixin.qq.com/s?__biz=MzIwMTc4ODE0Mw==&mid=2247484024&idx=1&sn=8dd29ba9a3eb22361b350f9be18db012&chksm=96e9ddf8a19e54eeaa8508ca462593668fe63c873de6fbf5d38f8d7ddb49b0787419cc0dd228&scene=21#wechat_redirect)
- [一种宋词自动生成的遗传算法及其机器实现-中国知网](http://kns.cnki.net/KCMS/detail/detail.aspx?dbcode=CJFQ&dbname=CJFD2010&filename=RJXB201003004&uid=WEEvREcwSlJHSldTTEYzVDhsOWpuU1lNU3Y4akU4VWtFTTg2SjZRT3pEYz0=$9A4hF_YAuvQ5obgVAqNKPCYcEjKensW4IQMovwHtwkF4VYPoHbKxJw!!&v=MDg3ODlEaDFUM3FUcldNMUZyQ1VSTEtmWStSdkZpam5XNzdBTnlmVGJMRzRIOUhNckk5RllJUjhlWDFMdXhZUzc=)

算法&模型：

- [PaperWeekly第36期---Seq2Seq有哪些不为人知的有趣应用？](https://mp.weixin.qq.com/s/B05RlAUfRWm_ECQRm_r40A)
- [从文本生成看Seq2Seq模型](https://zhuanlan.zhihu.com/p/29967933)
- [李智恒"GAN"法--对抗生成网络在文本领域的应用](http://ir.dlut.edu.cn/news/detail/442)


---

-update，180420
-created，180303