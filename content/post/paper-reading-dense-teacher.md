+++
title = "ECCV 2022 | 半监督目标检测新 SoTA, Dense Teacher"
date = 2022-07-15
lastmod = 2022-07-16T00:03:18+08:00
draft = false
katex = true
+++

这篇论文提出了一个新的 **半监督目标检测模型 Dense Teacher, 推翻了当前流行的用 thresholding 生成 hard pseudo label 的范式，Teacher 模型仅提供 dense pseudo label, 能够有效地提升单阶段目标检测器 FCOS 在半监督场景下的性能。COCO 10% labeled 情况下能达到 35.11 的 mAP，是当前半监督目标检测最好的效果。**


## 动机 {#动机}

{{< figure src="/ox-hugo/pngpaste_clipboard_file_20220715220246.png" >}}

**首先为什么当前的 Thresholding 生成 pseudo label 的方法不好？** 如上图所示，当前流行的 thresholding 生成 pseudo label 主要包含了三个步骤：

1.  NMS： 用一个 threshold 去除冗余的预测框；
2.  thresholding：用一个预定义的超参数 threshold 去过滤预测出来的框；
3.  label assign；

{{< figure src="/ox-hugo/pngpaste_clipboard_file_20220715220924.png" >}}

这三个地方都有对应的问题，前两个主要是对应的 threshold 很难选，如上图所示, (a)(b) 分别展示了 score threshold 和 NMS threshold 对模型性能的影响，可以看到模型的效果随着 threshold 的波动而剧烈波动。除此之外，这个最优的 threshold 在没有真正 label 的情况下几乎确定不了，大了不好，acc 上去了，但是 recall 很低，就会产生很多假阴性样本（False Negative）；小了也不好，recall 上去了，acc 低了，就会产生很多假阳性（ False Positive）样本。而且不同的检测器用不同的 label assignment 方法, 有噪声的 pseudo label 会严重影响 label assignment。之前 label assignment 也是单阶段目标检测器的一个研究方向，诞生了很多工作，比如 ATSS，PAA。


## 方法 {#方法}

基于上面提到的一些 thresholding 产生的问题，所以作者提出了 Dense Teacher，摒弃传统的 thresholding 策略。具体的的方法很简单，teacher 对整个 feature map 经过 sigmoid 生成一个 dense label（这里有些迷惑，但作者也没有给更多具体实现细节还有些缺失, 等放了代码再看）,有了生成的 dense label，就可以用 Quality Focal Loss （Generalized Focal Loss） 来监督 student 模型的输出了。

由于生成的 dense label 中间包含了很多 low score 的区域，作者提出用 FRS score 作为依据来过滤掉一些 low score 的区域。FRS score 定义如下：

{{< figure src="/ox-hugo/pngpaste_clipboard_file_20220715233343.png" >}}

其实就是某一个样本（point in FCOS）的分类最大概率值。然后 teacher 生成的 dense label 根据 FRS score 选取 top k% 来监督 student，其余不做约束。


## 结果 {#结果}

整个方法很简单，但是结果很有效。以 COCO-standard 结果为例（见下表）, Dense Teacher 在各种情况下都达到了最佳的效果。

{{< figure src="/ox-hugo/pngpaste_clipboard_file_20220715233703.png" >}}

除此之外，也对 dense label 的一些作用给出了一些解释，主要是说 dense label 能够找到一些 hard negative region 来辅助训练，如下图所示, dense label 与 box label 相比，会找到一些 hard negative region 来帮助模型训练（但是我感觉这个解释有点weak）。
![](/ox-hugo/pngpaste_clipboard_file_20220715233859.png)


## 总结 {#总结}

Dense Teacher 提出生成 dense pseudo label 来训练 student 模型，摒弃之前的 thresholding 方法，效果提升很明显。其实半监督目标检测里面的 thresholding 一直就有问题，所以设计了很多方法来选，但是检测和分类有很大的不同，检测是一个 box level 的 thresholding，合适的 threshold 非常难选，这篇文章给出了一个新的思路。但是作者只是很简单的说因为生成的是 dense label，所以选 dense object detector，也就是 FCOS 这类 anchor free 的方法。对于 two stage 的检测器，如 Faster RCNN 来说可能就不太适用。

BTW，CVPR 2022 有一篇 Unbiased Teacher V2，里面也使用了 FCOS 检测器，也复现了 Unbiased Teacher 的结果，但是二者结果相差还是很大的。举个栗子，COCO standard 10% 数据集下，unbiased teacher v2 复现的 FCOS unbiased teacher 只有 28.18, 而 Dense Teacher 复现的有 unbiased teacher 能达到 31.52 mAP(cls), 33.13 (cls + reg)， 这个结果和 unbiased teacher v2 的结果（32.61）差不多了。

对细节感兴趣建议阅读 paper 原文。
