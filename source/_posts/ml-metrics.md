title: Machine Learning Metrics
date: 2018-10-11 17:03:20
tags: [Machine Learning, metrics]
categories: Machine Learning
description: Machine Learning Metrics Summary. 
---

## Introduction

这里总结下机器学习中常见任务评价指标的计算方式以及他们代表的含义。通常线上使用的是业务指标，如点击率(CTR),转化率(CVR)等，线下使用机器学习指标，根据机器学习任务类型，可以分为分类指标，回归指标，聚类指标，排序指标等。

## 分类指标

在[Model Selection and Evaluation](http://frankchu.tech/2016/04/07/ML3/)中，我们介绍了Accuracy,Precision， Recall， F1，\\( F\_{\beta}\\)，confusion matrix(混淆矩阵)，这里我们补充下 `ROC`, `AOC`， `PR Curve`.


### PR Curve

以Precision为纵轴，Recall为横轴，根据分类threshold不同取值，绘制PR曲线，PR曲线越靠右上代表模型性能越好。


### ROC 与 AUC

在介绍ROC(Receiver Operating Characteristic)之前，我们先介绍TPR(真正率)和FPR(假正率).

$$ TPR = \frac{TP}{TP+FN} $$ 
$$ FPR = \frac{FP}{TN+FP} $$

ROC曲线就是以 TPR为纵坐标，FPR为横坐标，根据分类threshold的不同而做的曲线，而曲线下的面积即为AOC
(Area Under ROC Curve). 

ROC 曲线越靠近左上角表示模型性能越好，最好情况为(0, 1)点，即TPR = 1, FPR = 0, 进而推算出FN = 0， FP=0, 即全部预测正确。


### 对数损失 (Logistic Loss)

在[Linear Models for Classification](Linear Models for Classification)中，我们介绍二分类和多分类的对数损失即

$$ logloss = - \frac{1}{N} \sum_{i=1}^{N} (y\log p\_i + (1-y) \log(1-p\_i))$$

$$ logloss = - \frac{1}{N} \frac{1}{C} \sum_{i=1}^{N} \sum\_{j=1}^{C} y\_{ij} \log p\_{ij}$$


## 回归指标

在[Model Selection and Evaluation](http://frankchu.tech/2016/04/07/ML3/), 我们介绍了`MSE`, 常用的回归指标还有 

- `RMSE`  \\( RMSE = \sqrt(MSE) \\)
- `MAE`   \\( MAE = \frac{1}{N} \sum_{i=1}^{N} |f(x\_i) - y\_i|\\)


## 排序指标

在排序中，Precision， Recall， Accuracy， F1等指标也可以使用，但是这些指标没有考虑rank的顺序，在排序中常用的指标还有`MAP`, `MRR`, `NDCG`, `P@N`等

### P@N

Precision@N 的计算方式很简单，即对rank 出来的topN计算Precision。

### MAP

Mean Average Precision(平均准确率), 的公式分为两部分组成，先计算个体的平均准确率(Average Precision), 然后计算整体的平均准确率.

$$ AP@K = \frac{\sum\_{k=1}^{\min\_{M, K} P(k) Rel(k)}}{\min_{M, K}} $$

$$MAP@K = \sum_{q=1}^{Q} \frac{AP@K}{Q} $$

其中，M为IR返回的文档个数，K为要计算指标的文档个数， P(k)表示前k文档中的precision， rel(k)表示第k个文档和用户query是否相关。`MAP中的相关性只能用0，1表示`。

### MRR

Mean Reciprocal Rank(MRR)是常被用在QA和IR系统中的指标，它只关心正确的答案在rank出文档列表中的位置，不关心其他文档的出现位置，相应的， 

$$ MRR = \frac{1}{Q} \sum_{i=1}^{Q} \frac{1}{rank\_i}$$


### NDCG

Normalized Discounted Cumulative Gain(NDCG)是排序中经常用到的指标之一， 可以通过以下公式来计算：

$$ CG@K = \sum_{k=1}^{K} rel\_k$$
$$ DCG@K = \sum\_{k=1}^{K} \frac{2^{rel\_k}-1}{\log\_{2}^{k+1}}$$ DCG@K在CG@K的基础上引入了rank顺序惩罚
$$ IDCG@K = \sum\_{k=1}^{|REL|} \frac{2^{rel\_k} - 1}{\log\_{2}^{k + 1}} $$ IDCG@K 方便query之间进行比较，引入的normalization项，I(Ideal)指的是按照relevance从大到小的顺序进行排序，计算相应的DCG@K的值。
$$ NDCG@K = \frac{DCG@K}{IDCG@K}$$

---

## Reference

- [IR Evaluation Metrics](https://en.wikipedia.org/wiki/Evaluation_measures_(information_retrieval))
- [NDCG](https://en.wikipedia.org/wiki/Discounted_cumulative_gain)
- [美团机器学习实践](http://www.ituring.com.cn/book/2573)
- [CSDN Blog](https://blog.csdn.net/shenxiaoming77/article/details/72627882)
- [MRR](https://en.wikipedia.org/wiki/Mean_reciprocal_rank)
- [From zhihu](https://zhuanlan.zhihu.com/p/38850753)
- [机器学习](http://cs.nju.edu.cn/zhouzh/zhouzh.files/publication/MLbook2016.htm)

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

