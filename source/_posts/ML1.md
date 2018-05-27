title: Introduction
date: 2016-03-19 16:15:23
tags: Machine Learning
categories: Machine Learning
description: The reason why I write this series of blogs and some basic concepts of machine learning.
---

接触machine learning已经有一年多的时间了，在这段时间里，经历了从不懂到了解，从了解到理解，从理解到运用的几个阶段。起初开始学习maching learning的时候，一般以看为主，很少自己推倒，这样过了一段时间之后，发现之前看的算法基本都忘了。后来上[马毅][url1]老师GPCA课的时候，他曾告诫我们，要想把知识学好并变为自己的东西，最好的办法就是自己从头到尾推倒一遍. 在做数据科学国际大会[SSDS2015][url2]志愿者的时候，有幸负责接送[Eric Xing][url3]教授，在路上和他聊天的时候，Eric也指出学习maching learning一定要自己推倒，这是最基本的要求。在这之后的学习中，对所遇到的算法，都要自己推倒一番，发现效果很不错。

一直想整理machine learning方面的知识写成博客，但总是找不到太合适的时间。最近学院学分政策变了，我选了学院machine learning的课，加上最近在读周志华老师机器学习的[新书][url4]，便决定把之前学习的maching learning知识好好梳理一下。


## What is machine Learning?
Arthur Samuel (1959): "Field of study that gives computers the ability to learn without being explicitly programmed."

Tom Michel (1999): "A computer program is said to learn from experience E with respect to some class of tasks T and performance measure P, if its performance at tasks in T, as measured by P, improves with experience E."

My understanding：machine learning gives the computers the ability to learn, and according to different tasks, the computers learn different things in order to improve the performance. The machine learning algorithms can solve problems better than the algorithms used in the old days. 

**Exapmle: Recognizing handwritten digits.** 

Recognizing handwritten digits is not a nontrival problem due to the wide variability of handwritting. It can be tackled using handcrafted rules for distinguishing the digits based on the shapes of the strokes, but in practice such an approach leads to a proliferation of rules and of excpetions to the rules and so on, and invariably gives poor performance. However, using the machine learning methods can get a much better performance. [Example](http://yann.lecun.com/exdb/mnist/). In recent research, the use of deeplearning methods can reduce the error rate of recognizing handwritten digits down to less than 0.5%. 

## Why machine learning?

机器学习可以比较好的解决传统编程不能解决好的问题。就拿手写体识别这个例子来说，如果用传统编程的方式，我们很难设计手写体的全部规则并加以编程实现。但如果把这个问题用机器学习的算法来解决，实现起来比较简单，效果也好。
## Inductive learning

机器学习－“从样例中学习”是一个归纳过程，所以也称为归纳学习。通常我们假设整个样本空间(由特征张成的空间)服从一个未知的分布D，每个样本都是从这个分布上独立同分布的采样得到的。我们获得的样本越多，对样本空间的信息知道的就越多。训练集只是样本空间的一个很小的采样，但是我们希望它能够很好的反映样本空间的特性，否则在训练集上学到的模型范化能力会比较差。

我们可以把模型的学习过程看成是一个在由所有假设组成的空间中进行搜索的过程，搜索目标是与训练集相匹配的假设。但是在实际问题中，假设空间会很大，可能会得到很多个和训练集相匹配的假设(版本空间)。如何在版本空间中选择我们需要的模型？一种选择标准是“奥卡姆剃刀”。“奥卡姆剃刀”是一种常用的，自然科学研究中最基本的原则，即“若有多个假设与观察一致，选择最简单的那个”。但是在模型选择上，通常不好定义什么是“简单”。 另一个标准是归纳偏好(inductive bias)，即对某种类型假设的偏好。

## No Free Lunch Theorem (NFL)
NFL(没有免费的午餐定理)是说，无论算法a多聪明，算法b多笨拙，他们的期望性能是相同的。NFL有一个重要前提：所有问题同等重要。但有的时候我们只关心自己试图正在解决的问题，并不关心这个方案在其他问题上的效果。

NFL让我们认识到，脱离具体问题去空谈“什么学习算法好”毫无意义，因为若要考虑所有问题，所有算法都一样好。所以若要讨论算法的相对优劣，必须针对具体问题。在有些问题上表现比较好的算法，在其他问题上却可能不尽人意。

## Some concepts 

**Supervised Learning**

- Teach the computer how to do something, then let it use it;
- the data has labels
- example: linear regression，classification

**Unsupervised Learning**

- Let the computer learn how to do something, and use this to determine structure and patterns in data
- the data has no labels
- example : clustering

For more detailed examples for unsupervised and supervised learning, you can see [this][urllearn].

---

**Generative Models**

- Generative Models are used to model how data are generated. 

- For example, you are given a dataset and you plot the instances in the dataset. From the plot, you find the distribution look like Gaussian distribution. Then you choose Gaussian distribution as your model. Now, you think your dataset are genereatd from this Gaussian distribution. For each instance, there is a generating probability \\(p\_i\\), then the likelihood of generating the whole dataset is \\(\prod\_{i=1}^{N}p\_i\\), so we can learn the parameters by maxmizing the likelihood.

What if we cannot plot the dataset? In this case what kind of models should we use? Model selection can solve this problem well and it will be introduced in the following part.

- Another view : Generative Model: Modeling the joint distribution of all data. If the goal is to learn f: X --> Y, e.g., P(Y|X), generative models estimate parameters of P(X|Y),p(Y) directly from training data. Then we can use Bayes rule to calculate P(Y|X=x). Usually P(X|Y) can be related to the probability of generating the data given the model. 
(P(Y|X) = \\(\frac{P(X|Y)P(Y)}{P(X)}\\))

- Example: Mixtures of Gaussians


**Discriminative Models**

- Directly assume some functional form for P(Y|X)

- Estimate P(Y|X) directly from training data.

- There is a good saying for discriminative models: discriminative models don't do anything more than they are asked to do.

- Examples: SVM, Logistic Regression, Neural Network

--- 


## Model Selection

In order to choose the best model, we need to do model selection. If we have plentiful data, then one approach is to use some of the data to train some models, and then to compare them on independent data(validation set) and select the one having the best performance.

What if the supply of data is limited? We know that, in order to build good models, we wish to use as much of the available data as possible. So in this situation, we need to use **K-fold cross-validation**:

- Divide the dataset into K groups

- Take one group as test set, the others as training set.

- Repeat the last step, but this time choose another group as the test set. 

- Repeat like step 3 until all groups are used for test set.

- calculate the average performance tested from the above test sets.

- choose the best one

**Drwabacks:**
The number of training runs in cross-validation is K times comparing with no cross-validation. This can be problematic for models in which the training costs a lot of time.

## The Curse of Dimensionality


If the dimension of the data is too high, we will meet the following problems:

- The complexity of the models will increase as the dimensionality of the dataset increases.

- Our geometric intuitions can fail badly when we consider spaces of higher dimensionality. In spaces of high dimensionality, most of the volume of a sphere is concentrated in a thin shell near the surface.


# Reference

- Book: [Pattern Recognition and Machine Learning by Christopher Bishop](http://www.rmki.kfki.hu/~banmi/elte/Bishop%20-%20Pattern%20Recognition%20and%20Machine%20Learning.pdf)
- [notes of machine learning by Andrew Ng in cousera](http://www.holehouse.org/mlclass)
- GPCA(Generalized Principle Component Analysis) by [Yi Ma](http://yima.csl.illinois.edu/) (to be published)
- 龙星计划2010 slides by Eric Xing
- [机器学习][url4] 周志华


[url1]:https://scholar.google.com/citations?user=XqLiBQMAAAAJ
[url2]:http://ssds2015.shanghaitech.edu.cn/
[url3]:http://www.cs.cmu.edu/~epxing/
[urllearn]:http://www.holehouse.org/mlclass/01_02_Introduction_regression_analysis_and_gr.html
[url4]:http://cs.nju.edu.cn/zhouzh/zhouzh.files/publication/MLbook2016.htm
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>


--- 
