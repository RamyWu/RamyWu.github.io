---
layout: post
title:  "AI 自动写作 · 产品调研"
date:   2018-03-02 21:42:00 +0800
tags: AI-PM
category: Work
---

最近在做 AI 生成童谣的小项目，一直在做校对，对文本生成领域还不太熟悉，调研了一下这方面的产品和技术。


## 今日头条 xiaomingbot

- [拿下吴文俊奖，今日头条李磊谈AI如何实现内容创作_36氪](https://36kr.com/p/5109940.html)
- [AI小记者Xiaomingbot – 头条号(www.toutiao.com)](https://www.toutiao.com/m50050770286/?page_type=1)

做好内容引擎有四个环节

> 内容的创作、内容的推荐以及围绕内容的讨论还有内容的审核
> 

包含三方面的输入

> 实时比分、实时图片数据，以及热门比赛的文字直播。我们的机器人将这三方面融合起来，最后生成对应的文章。 
> 

Xiaomingbot 主要涉及的技术包括以下方面:


> 1. 是关于比赛的实时比分的数据通过文法结构和模板生成。
> 2. 关于图片，我们通过计算机视觉分析图片内容，将它和文字结合匹配出
> 3. 方面是知识库的建立，像比赛球队的历史、球员信息，作为额外信息补充进去。
> 4. 是网上有一些直播文字抓取过来的信息，通过机器学习里排序学习的技术去挑选最重要的内容，融合进文章中。
> 
> 网上的直播文字信息其实非常复杂，有不重要的信息，甚至会夹杂网友的评论。我们在生成新闻的时候希望把比赛最重要的环节，像进球、判罚等等给找出来；另外，需要考虑挑选出来的句子相互之间相似度要尽量小，但涵盖信息量又尽量大。通过 DPP 算法可以有效找出直播中的重点信息且涵盖最大的信息量。 
> 

机器写作方面我们面临的一些技术挑战。

> 一是深度内容自动生成有难度。算法能否自动从深度内容中学出模板以及良好的写作风格？
> 
> 二是机器写作不能仅关注生成，难点更在于对数据的分析和理解，比如哪些数据重要、数据之间的因果关系。这需要更好的算法来提高计算机的推理能力以及理解自然语言。
> 
> 三是目前写作还不能针对不同读者做到个性化。皇马和巴萨比赛，我是皇马球迷，你是巴萨球迷，我们关注的点可能不一样，我希望看到更多皇马的描述，你希望看到更多巴萨的细节。未来机器写作还需要做到个性化。


[如何评价今日头条研发的写稿机器人Xiaomingbot？ - 知乎](https://www.zhihu.com/question/49663523)

> 自动写作机器人，或者广义的说 文本生成，分为"模板填充式“，”抽取（chao xi）式”和“生成”式。模板填充式最好理解，有个模板往里面填项目，运动员比分就行。非常好做，甚至于大一大二的就能用.format("{0}比赛{1}夺得冠军", sports, player) 这种程序语句搞定。抽取式应该北大万小军老师在今年ACL2016上做到了顶峰 http://www.icst.pku.edu.cn/lcwm/wanxj/files/acl16_sports.pdf，一篇特征工程抽句子的文章，说的通俗点就是找一堆句子，我给拼成一篇文章。而真正的生成，现在还不可工业化，或者说生成新闻还不可工业化。这篇文章应该是基于模板的，也有可能是抽取式的，也就是找了几个报道这个新闻的文章，重新拼的


> 原因就是训练语料中判了之后都是死刑，其它有期徒刑的太少,而哈哈也是一个万能回复。所以，一个正确的机器的错误，应该是局部常见，但是放到句子中不对的短语。例如，在铜牌争夺战中，取得了胜利，赢得了金牌 这种错误。甚至有了斯坦福大学的人专门研究怎么减少这种废话生成的可能性，A Diversity-Promoting Objective Function for Neural Conversation Models综上，这个错误应该是非学习计算机自然语言的人，为了希望文章像机器写的，而故意露出的马脚，其实机器不会犯这么冒险的错误，机器可怂了



---

## 腾讯 Dreamwriter

- [如何看待腾讯推出的新闻写作机器人Dreamwriter？ - 知乎](https://www.zhihu.com/question/35513969)
- [机器人会写稿之后，腾讯还想让它实时整理出一份新闻简报](https://www.leiphone.com/news/201704/aNNpEpuCgAiQpBPB.html)
- [GitHub - rtfeldman/dreamwriter](https://github.com/rtfeldman/dreamwriter)

> 刘康认为，“机器写作从 0 到 1 大家都可以有，但从 1 到 2、到 3 要花费相当大的精力。”他表示“今天的数据量非常细，我们可以描述到每一个数据的颗粒还原。”
> 
> 他举例此前奥运期间的一篇跳水的稿件。“因为跳水是一个评分制的比赛，比如说我现在是评委，大家在进行比赛，我要给每个动作打分，这些打分都是记录在数据库里的，比如走板、空中姿态、落水姿态、水花这些都是有详实的数据的，它会被录进数据库。然后我们再怎么写它呢？这些数据会被我们通过一定的算法和机器自己的识别，我们先让机器跑几十万篇的数据，跑一个规则出来，它会自己把这些数据重新还原。因为每一个分数都可以还原成一个动作，这样通过一定的算法把它还原成原先的场面。”
> 
> 但是要做的很好，就非常难。刘康解释到，像财经和体育两个领域都难，但又不难。这是因为，财经本身有强烈的规则和算法模型，做简单的报道没有太多的技术含量，而难在对数据的解读、认知需要一套模型来判断它，甚至将来的预测。至于体育，关于球队比分可以从数据里抓取，但难在描述本身，“只有表示精彩才会看，如果不够精彩我会想去看视频。”



---

## 百度智能写作机器人 Writing-bots

- [智能写作机器人：不抢人类饭碗，我们只想人机协作 - 百度技术学院](http://bit.baidu.com/news/detail/id/62.html)

智能写作机器人与内容生产者之间主要有两层关系：

> 一是「代替」，将作者的重复性、规范性写作和客观数据聚合类劳动用机器进行替代，让作者可以更好的投入到深度文章的创作。
> 
> 二是「服务」，人工智能可以通过大数据帮助编写深度文章的作者，进行语料、素材的高效率搜集与初级加工工作，
> 
> 同时也可以基于行业的深度结构化数据完成基础数据分析及文章生成工作，服务于内容生产者，提升他们的写作效率。
> 
> 而这也正是我们智能写作的未来的发展目标——人机混合编辑。


从技术方案上看，主要分为两类：

> 一类是基于结构化数据、知识库或优质资源直接生成的文章。速报和大部分知识类文章是基于这类技术方案。这类文章因为直接从数据通过分析聚合或知识推理生成原始稿件，因此可以说是原创。
> 
> 另一类是在已有稿件的基础上通过内容分析聚合生成的新的文章。大部分资讯聚合类文章，如话题盘点、事件脉络、热门要闻回顾等都属于此类。因为是基于已有稿件内容创作新的稿件，因此可以看作是二次创作。当然，我们也可以在一篇文章的生成中结合上述两种技术，进行混合创作。

基本的创作流程是什么：

> 其中**核心流程「自动写稿」部分通常还包括文档规划（document planning）、微观规划（micro-planning）和表层实现 (surface realization) 三个阶段，分别解决稿件写什么、怎么写以及如何润色呈现的问题**。比如文档规划，需要确定写什么内容，采用什么结构来写，微观规划则更加细致化，具体要确定怎么来写每个段落、每个句子、每个标题以及内部的结构组织等。表层生成，则是对文章整体的润色和改写，比如如何调整文章格式、给文章配图等。


以资讯聚合类文章生成为例：


>  首先，聚合类文章的话题选择和资讯内容获取是基于内容理解和用户理解技术。利用百度自建的关注点图谱（主题、实体、事件标签以及标签间的关系）和标签预测技术，我们为每篇资讯内容打上关注点标签，同时根据用户的搜索或阅读行为可以获得用户的关注点标签，即用户的兴趣点。
> 
> 这样就获得了用户感兴趣的话题，同时基于内容标签可以获得相关话题的资讯内容。
> 
> 其次，基于内容理解和生成技术对于同一话题的内容进行压缩和聚合，相关技术包括：**事件分析，话题聚类，事件脉络抽取，自动摘要，标题生成、结构生成**等，而机器学习和知识推理是这些技术实现的基本方法。
> 
> 
> ![](http://ojcp18ifz.bkt.clouddn.com/2018-04-19-Snip20180420_731.png)

智能写作技术上最大的难点是什么：

> 人类作者在撰写文章时通常会对数据、内容和话题进行深入理解，并可以进行演绎、推理和联想，从而完成更深入的报道，充分表达自己的观点和立场。
> 
> 而相比之下，机器则更长于数据分析和规范的写作，在自然语言深入理解，以及让稿件具有观点和立场等方面还有很大的提升空间
> 
> 在深度文章写作方面，我们也在做一些探索和尝试，比如情感分析等研究，力图让机器写作更贴近人类，甚至希望有一天它能有自己的立场和观点，更加人性化。
> 
> 但就目前而言，我们的智能写作机器人会更聚焦用户需求，偏重数据分析和客观文章的撰写，致力于帮助内容创作者减少重复劳动，节省更多精力去撰写更加优质的深度内容。



---

## 微软 AI 对联&诗歌

[微软对联](http://duilian.msra.cn/)

> 微软对联是由微软亚洲研究院自然语言计算组研发的计算机自动对联系统。当用户给定上联，它能够自动提供若干下联供用户选择； 并且当用户确定一副对联后，它还能够生成若干四字横批供用户参考

对联：

![](http://ojcp18ifz.bkt.clouddn.com/2018-04-19-174422.jpg)

![](http://ojcp18ifz.bkt.clouddn.com/2018-04-19-174640.jpg)

诗歌：

![](http://ojcp18ifz.bkt.clouddn.com/2018-04-19-172437.jpg)

![](http://ojcp18ifz.bkt.clouddn.com/2018-04-19-172723.jpg)

---

## AI 科幻小说

- [AI写的科幻小说，人们会喜欢看么？_36氪](https://36kr.com/p/5109741.html)
- [Botnik Studios](http://botnik.org/content/harry-potter.html)
- [What Happens When an Algorithm Helps Write Science Fiction](https://www.wired.com/2017/12/when-an-algorithm-helps-write-science-fiction/)
- [Julian's Homepage](http://www.cs.toronto.edu/~jbrooke/)
- [Adam Hammond - Assistant Professor, Department of English, University of Toronto](http://www.adamhammond.com/)

![](http://ojcp18ifz.bkt.clouddn.com/2018-04-19-173302.jpg)

---

## AI 恐怖故事

- [Shelly：一个生产恐怖故事的 AI 机器人](http://www.ifanr.com/934408)
- [Shelley](http://shelley.ai/)
- [AI 人工智能 Shelley 的恐怖故事創作](http://www.ideamakerhk.com/ai/ai-%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD-shelley-%E7%9A%84%E6%81%90%E6%80%96%E6%95%85%E4%BA%8B%E5%89%B5%E4%BD%9C/)

>  Shelly 能够写出让人恐怖的故事。不过，它真正吸引人之处在于，这些故事不仅仅是算法的产物。在这个过程中，AI 强化和补充了人类的能力，而非替代人类的能力。Shelly 把人类思维融入到 AI 设计之中。
> 
> Shelly 的开发团队从 reddit 的恐怖故事板块提取了 1.4 万个恐怖故事，用来训练神经网络

![](http://ojcp18ifz.bkt.clouddn.com/2018-04-19-173517.jpg)

---

## 附：AI 自动写作 相关阅读&论文


- [足球赛事战报的自动写作研究 - 中国知网](http://kns.cnki.net/KCMS/detail/11.2442.N.20171105.1715.003.html)
- [NBA赛事新闻的自动写作研究 - 中国知网](http://kns.cnki.net/KCMS/detail/detail.aspx?dbcode=CJFQ&dbname=CJFDLAST2017&filename=BJDZ201702003&uid=WEEvREcwSlJHSldTTEYzVDhsOWpuU1lNU3Y4akU4VWtFTTg2SjZRT3pEYz0=$9A4hF_YAuvQ5obgVAqNKPCYcEjKensW4IQMovwHtwkF4VYPoHbKxJw!!&v=MTIyODdmWStSdkZpam5WYnJJSnlmUGRMRzRIOWJNclk5Rlo0UjhlWDFMdXhZUzdEaDFUM3FUcldNMUZyQ1VSTEs=)
- [PaperWeekly 第36期 --- Seq2Seq有哪些不为人知的有趣应用？](https://mp.weixin.qq.com/s/B05RlAUfRWm_ECQRm_r40A)
- [从文本生成看Seq2Seq模型](https://zhuanlan.zhihu.com/p/29967933)
- [李智恒 "GAN"法--对抗生成网络在文本领域的应用](http://ir.dlut.edu.cn/news/detail/442)

机器写诗

- [清华大学实验室作诗机器人“薇薇”通过图灵测试](http://www.guancha.cn/TMT/2016_03_21_354505_s.shtml)
- [PaperWeekly 第二十三期 --- 机器写诗](https://mp.weixin.qq.com/s?__biz=MzIwMTc4ODE0Mw==&mid=2247484024&idx=1&sn=8dd29ba9a3eb22361b350f9be18db012&chksm=96e9ddf8a19e54eeaa8508ca462593668fe63c873de6fbf5d38f8d7ddb49b0787419cc0dd228&scene=21#wechat_redirect)
- [一种宋词自动生成的遗传算法及其机器实现 - 中国知网](http://kns.cnki.net/KCMS/detail/detail.aspx?dbcode=CJFQ&dbname=CJFD2010&filename=RJXB201003004&uid=WEEvREcwSlJHSldTTEYzVDhsOWpuU1lNU3Y4akU4VWtFTTg2SjZRT3pEYz0=$9A4hF_YAuvQ5obgVAqNKPCYcEjKensW4IQMovwHtwkF4VYPoHbKxJw!!&v=MDg3ODlEaDFUM3FUcldNMUZyQ1VSTEtmWStSdkZpam5XNzdBTnlmVGJMRzRIOUhNckk5RllJUjhlWDFMdXhZUzc=)


---

- update，180420
- created，180302