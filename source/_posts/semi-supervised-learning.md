title: Semi-supervised Learning And Active Learning Summary
date: 2018-08-20 11:24:06
tags: [ml, semi-supervised learning, active learning, summary]
categories: Machine Learning
description: Semi-supervised Learning And Active Learning Summary.
---

## Introduction
在很多场景下(e.g., 业务冷启动)，labelled data是很少的，这时候会去尝试用无监督或者半监督的方式来解决实际中的问题。所谓半监督，就是既用了有标注的数据，又用了未标注的数据。
半监督学习和主动学习都是在标注数据少的情况下的一种解决方式。但是我们常说的半监督学习的方式偏向于不需要人工干涉，自动的对未标注数据进行利用。而主动学习(Active Learning)需要外在的相关领域人员进行数据标注，是一个人际协调交互的过程。

## Active Learning

原始数据量是很大的，但是怎么进行标注呢？对所有数据都标注一遍的成本是巨大的。一种方式可以使用`主动学习`的方式进行标注，减少标注成本。

`核心思想`：主动学习会选择那些比较难分类的样本，然后由人工进行标注。

`步骤`:

1. 从原始数据中随机采样，进行人工标注
2. 用标注的数据训练分类器，对未标注的数据进行预测
3. 挑选那些信息量大的样本进行标注， 对于二分类任务，选择那些概率在0.5附近的样本
4. 重复步骤2和3， 直到 
    - 没有更多数据进行标注
    - 当前分类器性能达到要求
    - 挑选出来的信息量大的样本，人工无法进行标注

## Semi-supervised Learning

Semi-supervised learning 包含的内容比较多，for more info，可以看周志华老师的西瓜书，这里只介绍常用到的自学习和协同学习。

### Self Learning 自学习 (Self Training)

`步骤`

1. 输入：原始未标注数据，部分标注数据
2. 用标注数据训练模型, e.g., 分类模型
3. 用训练好的模型原始数据进行预测
4. 从预测的数据中选择 最有把握的(e.g., 概率大于0.9) 加入到训练集中，并把他们从未标注数据集中移除
5. 重复步骤2-4， 直到
    - 模型性能达标
    - 数据集不再变化

### Co-Training 协同训练

Co-training 和self-learning的步骤类似，以下以二分类任务为例：

`步骤`

该算法假设数据属性拥有两个充分冗余（sufficient and redundant）的视图，称之为 view1 和 view2; view1, view2 一种对用的例子是特征的划分。

算法基本流程是：
1. 首先在标记数据集 L 的 view1 和 view2分别上训练出两个分类器 C1 和 C2；
2. 然后从未标记数据集 U 上随机的选取 u 个示例放入集合 U’中；分别用 C1 和 C2 对 U’中的所有元素进行标记；
3. 接着从两个分类器标记结果中各取可信度最高的 p 个正标记和 n 个负标记放入 L 中；
4. 最后从 U 中选取 2p+2n 个数据补充到 U’中；
5. 重复上述过程直到满足截止条件。

值得注意的是这两个视图应该是相互独立的。考虑一个极端的情况如果 view1 和 view2 是全相关的，那么由 view1 的到分类器和由 view2 训练得到的分类器对相同待标记示例的标记是完全一样的，这样以来Co-Training 算法就退化成了 self-training 算法。


## Reference
- http://lamda.nju.edu.cn/huangsj/dm11/files/gaoy.pdf
- https://blog.csdn.net/u014520745/article/details/45054481
- https://www.zhihu.com/question/265479171
- https://zhuanlan.zhihu.com/p/29583536
- https://blog.csdn.net/qq_35994754/article/details/73457817

