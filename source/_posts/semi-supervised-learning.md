title: Semi-supervised Learning And Active Learning Summary
date: 2018-08-20 11:24:06
tags: [ml, semi-supervised learning, active learning, summary]
categories: Machine Learning
description: Semi-supervised Learning And Active Learning Summary.
---

## Introduction
在很多场景下(e.g., 业务冷启动)，labelled data是很少的，这时候会去尝试用无监督或者半监督的方式来解决实际中的问题。所谓半监督，就是即用了有标注的数据，又用了未标注的数据。
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

## Self Learning 自学习

`步骤`

1. 输入：原始未标注数据，部分标注数据
2. 用标注数据训练模型, e.g., 分类模型
3. 用训练好的模型原始数据进行预测
4. 从预测的数据中选择 最有把握的(e.g., 概率大于0.9) 加入到训练集中，并把他们从未标注数据集中移除
5. 重复步骤2-4， 直到
    - 模型性能达标
    - 数据集不再变化

## Co-Training 协同训练



## Reference
- https://blog.csdn.net/u014520745/article/details/45054481
- https://www.zhihu.com/question/265479171
- https://zhuanlan.zhihu.com/p/29583536

