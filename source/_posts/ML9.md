title: Ensemble Learning
date: 2016-06-15 22:56:05
tags: [Machine Learning, Ensemble Learning, Boosting, Bagging, Random Forest, KNN, Decision Tree]  
categories: Machine Learning
description: Ensemble learning 常用来将若干个训练好的模型结合起来，以得到更好的performance。本文介绍了常用的ensemble learning算法： boosting， bagging 和 random forest，也介绍了常用在ensemble learning中的KNN 和 decision tree算法。
---

## Introduction

集成学习 (ensemble learning) 通过组合多个学习器来完成学习任务。相比于单个学习器，通过集成学习得到的模型往往可以取得更好的performance。因此在天池、kaggle和ImageNet等各大竞赛中，最终的模型往往都是通过集成学习得到的。

集成学习并不能保证集成后的模型一定比单个的模型效果好。如果各个学习器比较接近，那么集成后的模型不会有太多performance上的提升；如果各个学习器完全相同，那么集成后的模型不会有性能的提升。所以要想获得好的集成效果，需要各个学习器准确性不能太差，同时要有多样性，即“好而不同”。然而准确性和多样性本身就存在冲突。一般来说，准确性很高之后，要增加多样性就要牺牲准确性。所以如何产生并结合“好而不同”的学习器，也是集成学习研究的核心内容。

那么怎样将各个学习器组合起来呢？对于分类任务来讲，最简单的方法就是投票；对于回归任务来讲，可以通过加权平均的方式来组合各个学习器。那么有没有更好的做法呢？Boosting, bagging 和 random forest是常用的集成学习算法，看完下文对它们的介绍，相信你会有了答案。

## Boosting

Boosting是一种可将弱学习器提升为强学习器的算法。通常boosting的做法是：首先在原始数据上训练出一个基学习器；用这个基学习器来改变数据的分布，即增大在训练中出错样本的权重，使得出错的样本在后续的训练中受到更大的关注；减小未出错样本的权重；然后在调整分布后的样本训练集上训练新的基学习器；如此重复进行，直到基学习器的数量达到了预先设定的个数\\(T\\)，最终将这\\(T\\)个基学习器进行加权组合。

Boosting中最著名的代表就是adaboost (adaptive boosting)算法了：

### Adaboost 

输入：数据集 \\(D = \\{(x\_i, y\_1), (x\_2, y\_2), ..., (x\_n, y\_n) \\} \\), 基学习算法 \\(L\\)，训练轮数\\(T\\).

输出：集成后的学习器 \\(H(x)\\)

步骤：

1. 初始化训练数据权值分布：\\(W\_1 = \\{ w\_{11}, w\_{12}, ..., w\_{1n} \\}\\), \\(w\_{1i} = \frac{1}{n}\\), i = 1,2,3...n
2. for \\(t = 1 : T\\) do:
3. 在分布为\\(W\_t\\)的训练集上训练基学习器 \\(h\_t = L(D, W\_t) \\)
4. 计算基学习器 \\(h\_t\\)在训练数据上的错误率：\\(e\_t = \sum\_{i = 1}^{n} w\_{ti}  \ I(h\_t(x\_i) \neq y\_i)\\), 其中\\(I\\) 为 indicator function。
5. 计算基学习器加权系数：\\(\alpha\_{t} = \frac{1}{2} \ \ln\frac{1 - e\_t}{e\_t}\\)。 当\\(e\_t \leq \frac{1}{2} \\)时，\\(e\_t\\)越小， 加权系数\\(\alpha\_t \\)越大，在最终的模型中起的作用就越大。
6. if \\(e\_t > 0.5\\), then break;
6. 更新训练数据权值分布： \\(W\_{t+1} = \frac{W\_t \exp{(-\alpha\_t f(x) h\_t(x))}}{Z\_t}\\), 其中\\(Z\_t\\)为normalization factor。 展开来看有：若训练正确，则权重减小： \\(w\_{t+1, i} = \frac{w\_{t,i} \exp{(-\alpha\_t)}}{Z\_t}\\)； 若训练错误，权重增大： \\(w\_{t+1, i} = \frac{w\_{t,i} \exp{(\alpha\_t)}}{Z\_t}\\)
7. end for
8. \\(H(x) = sign(\sum\_{t = 1}^{T} \alpha\_t h\_t(x))\\), 其中\\(sign(x)\\) 为符号函数。

#### Adaboost 算法解释

Adaboost算法还有另一种解释，即认为损失函数为指数函数\\(E = \sum\_{i = 1}^{n} \exp{(-\frac{1}{2} y\_i \ F(x\_i))}\\), 其中 \\(F(x) = \sum\_{t = 1}^{T} \alpha\_t h\_t(x)\\)， 需要求的参数是\\(\alpha\_t\\)和各个基学习器中的参数。对于第\\(m\\)个基学习器来说，我们可以认为前\\(m-1\\)个基学习器是fixed的，所以可以通过优化损失函数来求得\\(\alpha\_m\\)和\\(h\_m(x)\\)中的参数。具体细节可以查看[PRML](prml)中的659-663页。

## Bagging

## Random Forest

## KNN

## Decision Tree

## Reference

- Book: [Pattern Recognition and Machine Learning by Christopher Bishop](prml)
- [Notes of machine learning by Andrew Ng in cousera](http://www.holehouse.org/mlclass)
- [机器学习][url1] 周志华
- Slides by [Prof. Wang](http://sist.shanghaitech.edu.cn/StaffDetail.asp?id=334)
- [统计学习方法](http://www.hangli-hl.com/books.html)
- [GPCA by Yi Ma]((http://yima.csl.illinois.edu/))

[url1]:http://cs.nju.edu.cn/zhouzh/zhouzh.files/publication/MLbook2016.htm
[prml]:http://www.rmki.kfki.hu/~banmi/elte/Bishop%20-%20Pattern%20Recognition%20and%20Machine%20Learning.pdf

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>