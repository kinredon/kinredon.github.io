+++
title = "参加VALSE 2021的几点总结"
date = 2021-10-12
lastmod = 2021-10-13T00:52:02+08:00
tags = ["summary", "VALSE"]
categories = ["summary"]
draft = false
katex = true
+++

## 前言 {#前言}

很幸运能有机会前往杭州参加 VALSE 2021，之前就一直关注 VALSE 的线上活动，也在线听了不少 VALSE 的 webinar，都是计算机视觉各个研究方向的优秀青年学者来做讲座。VALSE 2021 年度大会，邀请了很多大佬做 tutorial 和 workshop 的讲解，整体参与下来，也算收益良多。接下来简单聊一下我的感想做几点总结。


## 几点总结 {#几点总结}

****徐宗本院士：如何突破机器学习的先验假设？**** 起初我只当这是一个普通的讲座，但从一开始就感受到了院士的气场，自信而不张扬，听出了其对机器学习的深刻认识，瞬间激起了我的兴趣。讲座一开始给出了一个大一统的公式指出现有机器学习范式主要包括五个部分：模型假设空间、损失函数、正则项、数据空间、优化算法，并指出了各个部分的局限性，最终针对这五个部分给出了一些解决方案，相应的探索也有些突破。很多部分之前都没听说过，但不明觉厉，这是真正的具有前瞻性的研究，令人大开眼界。

****迁移学习年度进展汇报**** 由于我的主要研究方向是迁移学习，所以我对此方向的汇报关注都比较高。之前听过张磊老师的报告，对迁移学习领域自适应方向的讲述很全面。这次年度进展汇报给予了新的惊喜，张老师给出了十个迁移学习当前的研究方向，主要如下图所示：
![](/ox-hugo/pngpaste_clipboard_file_20211013000524.png)
有些工作之前关注过，但却没有进行系统的梳理，但这对整个领域的认识还是很重要的。后续有机会对每一个方向进行细致的讲述，不知道有没有人看。

****Vision Transformer 锋芒毕露，势不可挡**** 此次大会 Vision Transfomer 占有很大比重，tutorial 和 workshop 都有，如香港大学罗平老师的《Vision Transformer for Object Detection and Scene Segmentation》和曹越老师的 Transformer tutorial。Vision Transfomer 之前给我的感觉仅仅是计算机视觉一个新兴的研究方向，很火但还很初级，虽然听过 Swin Transfomer 这样大杀器，知道重要性，但一直没有认真地关注它。但此次大会发现其开始在各个领域大放异彩，backbone网络的 VIT，CMT，Swin等，各个 high level 视觉任务的突破如检测 DETR，分割里面的 segformer 等，以及在视觉任务竞赛中现有很多 Top 解决方案采用开始采用 Vision Transformer。本次大会让我更加地重视 Vision Transfomer，锋芒毕露，势不可挡。总的来说，Vision Transformer对深度学习意义重大，赶快进军 Vision Transfomer。[为何Transformer在计算机视觉中如此受欢迎？](https://www.msra.cn/zh-cn/news/features/cv-transformer)

****算力支撑起来的大规模预训练模型**** Google Brain 研究员翟晓华指出大规模数据、大模型、超长 training schedule 相辅相成，能给得到极好的预训练模型，大大提升模型在fintune后的性能，其中最受震撼的就是，同一个任务8张GPU训练一周和一个月性能的显著差距，所以算力才是 yyds。

****下游任务的无监督表征学习**** 近期无监督表征学习得到广泛地研究，利用自监督方法让模型学习到更好的表征，主要包括 pretext task 和 contrastive learning。pretext task 利用数据手动创造一些标签用于给模型额外的监督信号，如旋转角度预测，patch排序，mask 预测等。contrastive leaning 自 moco 以来出现了很多有意思的工作，利用对比的思想，构造正和负样本对，拉近正样本对的距离，分离负样本对，实现鲁棒特征的学习。这些通过无监督表征学习的得到的特征更具判别力，在 imagenet 上进行 linear classification 的精度不断提高。然而目前这些工作对下游任务如检测、分割的提升非常有限，然而这些任务又极其重要，因此现在有很多 contrastive 的方法用于这样 dense prediction 任务的情况，比如 pixel-to-propagation 用来做分割。

****Poster & AI 公司**** 很喜欢 Poster 这一环节，可以去看自己喜欢的 paper，有问题能够直接和作者面对面交流，也能认识很多大佬。VALSE 的赞助商很多，包含国内很多优秀的 AI 公司或者实验室，直接去他们摊位交流还是很有效的。观察来看，自动驾驶公司感觉还是蛮多的，其次是各大公司的实验室，以及初创公司。大家为了抢人使出浑身解数，感觉最有效地方式就是抽奖了，这次感觉极视平台下了血本，服务器都给抽了：）就是个人运气不太好，看来永远不能成欧皇了哈哈哈

****其它**** 这篇总结主要是在飞机上用手机凭记忆打出，听的时候没有拍照记录，所以很多细节记录不清。因此还有一些有意思的讲座上面没有提及，包括黄高老师的动态网络、崔鹏老师的OOD，stable learning，彭玺老师的 contrastive clustering 等等，希望后续 VALSE 能够 release slides，可以及时复习。其它有意思的讲座有很多，但受限于个人精力，仅看了部分的讲座。整个 VALSE 大会对我来说唯一遗憾的是，后面的 tutorial 和 workshop 没有迁移学习的 section，很多工作都穿插在了自监督和无监督的讲座里面。
