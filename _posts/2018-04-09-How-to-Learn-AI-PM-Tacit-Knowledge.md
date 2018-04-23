---
layout: post
title:  "AI PM 有哪些需要掌握的内隐知识？"
date:   2018-04-09 19:12:00 +0800
tags: AI-PM
category: Work
---


这个问题的背景是...好闺蜜刷了一段时间的 AI PM 相关的文章、公众号，还是没法理解这些知识和概念，问我如何学习 AI 产品经理所需的内隐知识...

## 有哪些内隐知识

从 AI PM 的日常工作来看，最多内隐知识的情况：

### 团队沟通

首先是项目管理中每天各种团队沟通和协作，这部分不仅仅是需要沟通技巧，更多的是对问题的理解，还有团队职责划分的理解。

比如 PM 经常要召开会议的例子：团队人员又有算法工程师，技术、产品，设计，领导，...如何开会？有哪几种会议？会议流程，会议方式？会议之前的准备...

首先要清楚正式会议的目的，不是几个小时漫长的讨论，而是在已经通过确认的基础上，决策后的通知，包括提出可能存在的问题和风险，...

所以在开会之前，是否已经得到大家对需求的共识、理解和领导的确认，是否对本次会议涉及的执行者、提前进行过需求和问题的沟通。因为有很多地方是需要私下和同事确认和同步的，有一些关键决策也要提前找老大审核才能开大会，还有平时的各种小会，需要有谁在，不需要谁在，什么时候叫什么人，每个人的职责和角色，部门同事的上下游关系，谁需要决策，谁负责执行，谁需要被通知等等...PM 要很清楚这些事情。但是这些东西没有书或者课程来教，都是从工作中去观察和思考。

所以，沟通的目的都是为了达成一致，解决，落地到具体事情，帮助整个项目往前推进。

### 按时交付-控制项目进度

然后是对产品在开发周期的控制问题。首先在需求理解的过程中，对需求背后的用户场景和客户目标的理解、然后将需求拆解成多个可执行的任务，比如项目很紧，客户ddl日期就在2个月后做Demo测试。

这就需要 AI PM 非常严格地控制项目进度，在客户的 ddl 之前完成对方需要的demo，并把握基本质量。除了时间观念，更重要的是风险控制，因为从设计到开发的中间会有各种问题的，就需要产品经理跟踪，参与，甚至自己身体力行地去做一些事去推进整个项目进展。

之前做的一个比较大的 AI 项目，大概会有这么些流程和进度控制：

> ![](http://ojcp18ifz.bkt.clouddn.com/2018-04-23-WechatIMG620.jpeg)
> ![](http://ojcp18ifz.bkt.clouddn.com/2018-04-23-WechatIMG621.jpeg)



### 数据标注和算法原理的基本常识

这些可以说是外显知识，但是最开始要理解常识，也要从项目具体行动中去做，去学，才能理解背后的规则的，才能真正掌握这些知识。

举个例子，我之前在写标注文档的时候发现，标注团队会问我很多问题，先不说我要花很多时间去咨询算法工程师，每一个 case 都在问特殊情况要怎么标，浪费时间之外也导致标注员大批量标错导致返工浪费很多时间。并且，AI PM 要对标注规则背后的算法原理和用户真实需求的理解，才能把产品做得更好。

比如我做 OCR 的时候标注一个营业执照出现图片模糊不知道怎么标点的这个例子：

（1）需要理解，机器识别和肉眼识别的能力边界，如果不理解的情况下导致大批量标错，就会导致算法的质量下降 or 测试结果不准确。

> ![](http://ojcp18ifz.bkt.clouddn.com/2018-04-23-112905.jpg) 


（2）理解用户真实场景中有各种不同的数据和可能，这些边界问题有哪些，算法应该给出什么样的结果和定义。

> ![](http://ojcp18ifz.bkt.clouddn.com/2018-04-23-113108.jpg)


... 还有很多，比如对问题的分析和思考，对产品方案的选择和判断...，也是需要深入理解后才能做出决策。


## 如何学内隐知识

可以看到 AI PM 这个岗位，除了硬技能，还有很多内隐知识是需要通过自己实战项目的行动学习、通过具身认知去提炼这些问题，才可以习得并理解的。

所以，如果 PM 新手光直接看文章，大部分的情况就是，看了也不知道怎么去思考这些问题，问题背后是为什么？没有方向，漫无目的的学习，死记硬背这些概念，是没有意义的。

另一方面，内隐知识无法充分地用被言语所表达（这是内隐知识的性质，--维基百科），文章里面提到的东西背后其实有很多不可描述的东西，作者也很难将经验转化让小白能理解并掌握，所以他们看完后思考的问题，也只能是很表面的问题。

已经在 AI PM 这条路死磕过的人来说，自己每次遇到许多的问题->寻找的解决问题的思路->解决问题，在自己解决具体的问题的过程中，会有一个对这个行业、岗位的整体或具体的了解，通过这些问题背后的各种原则，提炼出对这个行业岗位的一些方法论。而这个过程，又涉及到很多商业敏感信息，无法放到博客或公众号写出来给小白学习，所以...大家看到的也就是流失了80%精髓的内容。

所以要学 AI PM 的内隐知识，先关注工作本身和产品的细节和过程经历吧，已经成为 AI PM 的小伙伴们自己去用心体会，每日好好写工作总结。

如果即将成为 AI PM，可以通过预备项目来学习，看案例，理解场景，找到哪些关键问题，看相关论文，写一份详尽的分析报告，这样都比盲目刷资料学的深入一些。还可以适当参加一些 AI PM 社群，和大家一起做项目，有交流和讨论也比自己挖坑的感觉好多了

---

## 学习方法

最后额外说一下关于「如何学习」的方法问题。

我以前觉得学习好像是为了提高自己的能力...但是发现有时候看起来是在「学习ing」，但是并没有提高xx能力，后来才觉悟出xx能力是在行动和思考中提高的啊，不是在「学习ing」提高的。当然，学习分为很多种学习，比如：主题学习、深度学习（不是机器学习的那个深度学习网络哦）、行动学习...

为什么很多人听过很多道理依然过不好这一生？就是因为没有行动！通过行动、刻意练习，将这些知识转化为自己的内隐知识。带着问题去读书找资料，少在「学习」面前走弯路。

> 学习的本质是改变，不管是认知上的改变还是行为上的改变。
> 
> [如何生成自己的知识树-开智部落](https://mp.weixin.qq.com/s?__biz=MzI1NjQ5NzM2Ng==&mid=2247483735&idx=1&sn=f5112ee2c89322a019fd683d43b68a8b&chksm=ea2482eedd530bf85c888586fb3e7f173953defd13dc2968e6543ff848cd691fc051f844d2fb&mpshare=1&scene=1&srcid=0408NymNffoPKoTaXFTjNmBN&key=52f65e2fc335f081aa98694a8dae7966b8a6e13b2ead07d1258694cfd5a57b52ff2f276f4aad4e97a056e447e59cff9cb702c58ac3976eb1b1acfc6f448855a88eb554d3caac3a707916e96f4e18fe30&ascene=0&uin=OTYyNDg4NjIx&devicetype=iMac+MacBookPro14%2C1+OSX+OSX+10.12.5+build(16F2073)&version=12020810&nettype=WIFI&lang=zh_CN&fontScale=100&pass_ticket=XhQRlddtEjj7fjqXNaZGiSbN4xHzM0uG6x%2BZLQH%2B7Op7yAMWXAd5X%2Fyp3FGshV63)

最后还是不自觉地想起了我老板阳老师的旧文：元认知学习法系列，元认知，就是对思考的思考，思考之前，先学习如何思考。

- [工作谈016：元认知学习法（1）](https://mp.weixin.qq.com/s?__biz=MzA3MzM0MjUyMQ==&mid=2652149229&idx=1&sn=f17e909f3aea79267c4492d9786df711&mpshare=1&scene=1&srcid=0404mrpusTIqetasxY47Xi5T&key=81c79c138bc9861d15d81b5e491cd8d621c670d900aebe9f010122569692bd40ad6e47eae99658990ca600acd2499566c762f2deb72fea792fd7c76a21542d0b3cbfb4a9af8f40859b31b2a30c1e28bb&ascene=0&uin=OTYyNDg4NjIx&devicetype=iMac+MacBookPro14%2C1+OSX+OSX+10.12.5+build(16F2073)&version=12020810&nettype=WIFI&lang=zh_CN&fontScale=100&pass_ticket=MkaafPgVreQBUzTk1rOeYHeCpElwPtCQW7eOf1FNlxJi%2FU0xRl6DtleTLVfXbMwK)
- [工作谈017：元认知学习法（2）](https://mp.weixin.qq.com/s?__biz=MzA3MzM0MjUyMQ==&mid=2652149232&idx=1&sn=3709475a3a4b6fc2fb56f7e25a52d1b7&mpshare=1&scene=1&srcid=0404gHrU7N7SGE0BQAuw3U9o&key=52f65e2fc335f08165de0837df128c8819a0395d4dae85f4b1c8589b62f7427b8e348c3af927f3f45ba72b8f4a5bde4c0c7cfd1ea4823b5505bf106867095ecfa8de409f8ce57fa684f421c3066f22e2&ascene=0&uin=OTYyNDg4NjIx&devicetype=iMac+MacBookPro14%2C1+OSX+OSX+10.12.5+build(16F2073)&version=12020810&nettype=WIFI&lang=zh_CN&fontScale=100&pass_ticket=MkaafPgVreQBUzTk1rOeYHeCpElwPtCQW7eOf1FNlxJi%2FU0xRl6DtleTLVfXbMwK)
- [工作谈018：元认知学习法（3）](https://mp.weixin.qq.com/s?__biz=MzA3MzM0MjUyMQ==&mid=2652149235&idx=1&sn=c1f678d649fae0ebc25380fd041e5cb7&mpshare=1&scene=1&srcid=0404UNqi3eHLGaXpLodeXgew&key=81c79c138bc9861d2bf92f9928890ac172ebc30de62e9e1e9dfccd5489eea114d32715313b3dcc570734f738bb46db4c2d73af9429a3c6f82da9f35104b78cae256eb70945656c7c8b3569b5d569afcc&ascene=0&uin=OTYyNDg4NjIx&devicetype=iMac+MacBookPro14%2C1+OSX+OSX+10.12.5+build(16F2073)&version=12020810&nettype=WIFI&lang=zh_CN&fontScale=100&pass_ticket=MkaafPgVreQBUzTk1rOeYHeCpElwPtCQW7eOf1FNlxJi%2FU0xRl6DtleTLVfXbMwK)
- [工作谈019：元认知学习法（4）](https://mp.weixin.qq.com/s?__biz=MzA3MzM0MjUyMQ==&mid=2652149239&idx=1&sn=aac4a6eadc3b9390455206df7c9a1543&mpshare=1&scene=1&srcid=04042XorepQt77szrdqIMh04&key=8e58dd8e1c344716b0b2d48f8d7981eccba67673612f89f37f8f6e4147a8a6fe2d269b6425494f1386d4619fe544493fbec7ae5dfeec332fc4941166920f0fc3ab75eed919071390f23f47c909f355f1&ascene=0&uin=OTYyNDg4NjIx&devicetype=iMac+MacBookPro14%2C1+OSX+OSX+10.12.5+build(16F2073)&version=12020810&nettype=WIFI&lang=zh_CN&fontScale=100&pass_ticket=MkaafPgVreQBUzTk1rOeYHeCpElwPtCQW7eOf1FNlxJi%2FU0xRl6DtleTLVfXbMwK)
- [工作谈020：元认知学习法（5）](https://mp.weixin.qq.com/s?__biz=MzA3MzM0MjUyMQ==&mid=2652149243&idx=1&sn=4265d0fb8747c5c8d633bc9dce48a7a1&mpshare=1&scene=1&srcid=04044sjKfX2LVCIwwgeYr26O&key=81c79c138bc9861df426ab786e962641c87abfc0bd24cc5ec44088b63010a3339e26a13ac23d5447e45cd8b91b31036a6afb1d5944a3be7f106eb170aecd244b1c0216f8da6abbf3a1440300b5847510&ascene=0&uin=OTYyNDg4NjIx&devicetype=iMac+MacBookPro14%2C1+OSX+OSX+10.12.5+build(16F2073)&version=12020810&nettype=WIFI&lang=zh_CN&fontScale=100&pass_ticket=MkaafPgVreQBUzTk1rOeYHeCpElwPtCQW7eOf1FNlxJi%2FU0xRl6DtleTLVfXbMwK)

成为聪明的学习者，不要盲目地学习。

---

- created，180409