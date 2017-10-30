title: Clustering
date: 2016-05-15 22:56:05
tags: [Machine Learning, Clustering]  
categories: Machine Learning
description: 聚类 (clustering)是机器学习中经常会涉及到的一个task, 本篇文章从k-means开始讲起，然后到Mixtures of Gaussians和EM算法，最后介绍层次聚类、密度聚类和Learning Vector Quantization (LVQ)。
---

# Clustering

聚类 (clustering)是机器学习中经常会涉及到的一个task。通常来讲，它是一种unsupervised task，通过挖掘数据内部的结构等信息，来将数据划分成若干部分，也有利用数据的label信息来做辅助的方法，如Learning Vector Quantization (LVQ)。

## K-means

提到聚类，最常用的方法就是K-means了。K-means的思想比较简单：对于一组数据，我们希望找到一组中心点，使得整个数据集中所有点到离它最近的中心点的距离之和最小，相应的数学表达式为：

$$ E = \sum\_{i=1}^{k}\sum\_{x \in C\_i} ||x - \mu\_i||\_2^2$$

其中，\\(k\\)为簇的总数，\\(C\_i\\)为相应的第\\(i\\)个簇。若直接对\\(E\\)进行求解很困难(NP-hard)，因为有指数多个可能的簇划分。所以这里采用迭代优化的方法来近似求解。

1. K-means首先初始化k个中心点，这里需要提到的是k-means的初始化对最终结果有着很大的影响，不同的初始化会有着不同的聚类效果。
2. 对数据集中的所有点，找到距离它最近的中心点，这样就得到了新的簇的划分。相应的数学表达式为：\\(i = \arg\min\_{i=1,2,...,k} ||x - \mu\_i||\_2^2\\)
3. 在新形成的k个簇中，更新k个相应的中心点。相应的数学表达式为：\\(\mu\_i = \frac{\sum\_{x \in C\_i} \ x} {|C\_i|}\\)。
4. 重复步骤2和步骤3直到达到迭代次数或者收敛 (E的值变化很小)。

### k-means中存在的两个问题

k-means中有两个问题需要值得注意：

1. k-means中的中心点怎么初始化 
2. k-means中的k怎么选取

#### k-means中心点初始化
对于中心点的初始化，我们希望初始化的中心点能在数据点附近，同时中心点尽量分散开。 如果中心点初始化的比较近的话，很有可能就都初始化在同一个簇中，这样就使得原本应该划分为同一个簇的数据点被划分成了两个簇。

通常的做法有：

1. 随机初始化中心点到数据集上的k个点上。然后跑若干次，选取其中\\(E\\)最小的那一组初始化。这种做法的缺点在于，有可能两个中心点离的很近。
2. 首先随机初始化一个中心点到数据集上的一个点上，第二个中心点初始化为距离第一个中心点最远的那个数据点；对于后面的中心点的初始化，我们每次初始化成所有数据点中距离前面初始化好的中心点最近距离最大的那个点。
3. 用k-means++方式来初始化。k-means++的初始化方法与第二种方法比较像，不同之处在于k-means+＋引入了概率，即每一个数据点成为下一个中心点的概率正比于该数据点与其最近中心点距离的平方\\(D(x)^2\\)。

#### k-means中的k怎么选取
当k取值越大时，相应的E会越来越小，如果k取为数据点的总数，那么E会变成0。所以单独的E不能作为k选取的标准。一种解决方法是用\\(E+complexity\\)来作为衡量的标准，其中complexity与k的大小有关，k越大，相应的complexity越大。常用的衡量标准有 BIC (Bayesian Information Criterion)。还有一个在聚类中经常用到的衡量标准：CH (Calinski-Harabasz) index，有兴趣的读者可以自行google，这里不做具体介绍了。

## Mixtures of Gaussians
k-means 在某种条件下，可以看作是mixtures of Gaussians的特例。在k-means中，我们认为每一个数据点必须属于某一个簇，这个条件太硬(hard)了， 在mixtures of Gaussians中，我们认为每一个数据点按照某个概率属于某一个簇。

### Introduction

数据通常满足一定的分布，对于比较简单且满足高斯分布的数据，我们可以用一个高斯分布来对这些数据来建模。然而，现实中数据的分布通常比较复杂，只用一个高斯分布建模效果不好，通常用若干个高斯分布的组合来建模数据的分布，这里我们用K个高斯分布的组合来建模数据的分布：

$$p(\mathbf{x}) = \sum\_{k = 1}^{K} \pi\_{k} N(\mathbf{x}|\mu\_{k}, \Sigma\_{k})$$ 其中\\(\pi\_{k}\\)为高斯分布\\(N(\mathbf{x}|\mu\_{k}, \Sigma\_{k})\\)的加权系数。

### Hidden Variables

我们引入一个K维的hidden variable \\(\mathbf{z}\\)， \\(\mathbf{z}\\)是一个one-hot的向量，用来建模当前的变量\\(\mathbf{x}\\)由哪一个高斯分布生成的 (知道这个信息后，模型的表示和参数求解都会变得简单)，相应的， $$p(\mathbf{x}) = \sum\_{\mathbf{z}} p(\mathbf{z}, \mathbf{x}) = \sum\_{\mathbf{z}} p(\mathbf{z})  p(\mathbf{x}|\mathbf{z}) ＝\sum\_{k = 1}^{K} \pi\_{k} N(\mathbf{x}|\mu\_{k}, \Sigma\_{k})$$

### Cost Function

Mixtures of Gaussians是生成模型，我们用maximum likelihood estimation (MLE)来求得我们的优化目标，即：

$$ \log L(\mathbf{\pi}, \mathbf{\mu}, \mathbf{\Sigma}) = \log(\prod\_{i = 1} ^ {N} \sum\_{k = 1}^{K} \pi\_{k} N(\mathbf{x}\_{i}|\mu\_{k}, \Sigma\_{k})) = \sum\_{i = 1}^{N} \log(\sum\_{k = 1}^{K} \pi\_{k} N(\mathbf{x}\_{i}|\mu\_{k}, \Sigma\_{k})) $$

模型的参数为\\(\mathbf{\pi}, \mathbf{\mu}, \mathbf{\Sigma}\\), 我们通过优化目标函数来求得最优的参数值。那么如果有了学好的模型，我们怎么做prediction呢？我们可以通过计算\\(p(z\_k = 1 | \mathbf{x})\\) 的概率，然后将点\\(\mathbf{x}\\)划分到概率最大的那个簇，相应的：
$$ p(z\_k = 1 | \mathbf{x}) = \frac{p(z\_k = 1) p(\mathbf{x}|z\_k = 1)}{p(\mathbf{x})} = \frac{\pi\_k N(\mathbf{x}|\mu\_{k}, \Sigma\_{k})}{\sum\_{k = 1}^{K} \pi\_k N(\mathbf{x}|\mu\_{k}, \Sigma\_{k})}$$

接下来的问题就是怎么优化目标函数来求得最优参数的值了，对于含有隐变量的优化问题，我们通常用Expectation Maximization (EM)来求解。
 
## Expectation Maximization (EM)

EM算法是一种常用的优化算法，多用在含有隐变量的优化问题上。通过MLE，我们可以得到相应的优化目标，即：

$$ \log L(\mathbf{\theta}) = \log \prod\_{i = 1}^{N} p(\mathbf{x}) = \sum\_{i = 1}^{N} \log p(\mathbf{x}) = \sum\_{i = 1}^{N} \log \sum\_{\mathbf{z}}p(\mathbf{x}, \mathbf{z}) = \sum\_{i = 1}^{N} \log \sum\_{\mathbf{z}}p(\mathbf{x}, \mathbf{z}) $$

由于对数函数为凹函数，则由Jesen不等式，我们可以得到：

$$\log L(\mathbf{\theta}) = \sum\_{i = 1}^{N} \log \sum\_{\mathbf{z}\_i}p(\mathbf{x}\_i, \mathbf{z}\_i) = \sum\_{i = 1}^{N} \log \sum\_{\mathbf{z}\_i}Q(\mathbf{z}\_i) \frac{p(\mathbf{x}\_i, \mathbf{z}\_i)}{Q(\mathbf{z}\_i)} \geq   \sum\_{i = 1}^{N} \sum\_{\mathbf{z}\_i}Q(\mathbf{z}\_i) \log \frac{p(\mathbf{x}\_i, \mathbf{z}\_i)}{Q(\mathbf{z}\_i)} $$

其中，\\(Q(\mathbf{z}\_i)\\) 为关于隐变量\\(\mathbf{z}\_i\\)的一个分布。经过放缩后，对于任意一个\\(Q(\mathbf{z}\_i)\\)，我们都可以得到目标函数的一个下界，那么怎么选取\\(Q(\mathbf{z}\_i)\\)呢？对于特定的参数\\(\mathbf{\theta}\\), 我们可以选取\\(Q(\mathbf{z}\_i)\\), 使得上述不等式关系中的相等关系成立。由Jesen不等式可知，当且仅当\\(E(f(x)) = f(E(x))\\), 等式成立，即 \\(\frac{p(\mathbf{x}\_i, \mathbf{z}\_i)}{Q(\mathbf{z}\_i)} = c\\), 相应的，\\(Q(\mathbf{z}\_i) \propto p(\mathbf{x}\_i, \mathbf{z}\_i)\\). 又由\\(\sum\_{\mathbf{z}\_i} Q(\mathbf{z}\_i) = 1\\), 得到\\( Q(\mathbf{z}\_i) = p(\mathbf{z}\_i | \mathbf{x}\_i, \theta)\\).

由此得到EM算法: 首先我们初始化参数\\(\theta\\), 然后经过E-step 和 M-step若干次迭代，直到收敛。

- E-step: For each i, set

$$ Q(\mathbf{z}\_i) = p(\mathbf{z}\_i | \mathbf{x}\_i, \theta) $$

- M-step, set:

$$ \theta = \arg \max\_{\theta}  \sum\_{i = 1}^{N} \sum\_{\mathbf{z}\_i}Q(\mathbf{z}\_i) \log \frac{p(\mathbf{x}\_i, \mathbf{z}\_i)}{Q(\mathbf{z}\_i)} $$

### Convergence

EM算法一定会收敛吗？答案是肯定的。因为在E-step中，\\(\log L(\theta)\\)等于其下界的值；在M-step中，我们最大化下界来获得新一轮的参数\\(\theta\\). 然后在下一次的E-step中，\\(\log L(\theta)\\)又等于新的下界的值。所以在EM算法中，优化目标是不断变大的。但是EM算法只能保证局部收敛；如果优化目标是凸函数，则可以得到全局最优解。

### Mixture of Gaussians revisited

有了上面EM的推倒，相信现在可以轻易的推出Mixture of Gaussians中参数更新的公式了：

- E-step:

$$ \gamma\_{k} = p(z\_k = 1 | \mathbf{x}) = \frac{\pi\_k N(\mathbf{x}|\mu\_{k}, \Sigma\_{k})}{\sum\_{k = 1}^{K} \pi\_k N(\mathbf{x}|\mu\_{k}, \Sigma\_{k})}$$

- M-step:

$$\mu\_{k} = \frac{\sum\_{i=1}^{N}\gamma\_k \mathbf{x}\_i}{\sum\_{i=1}^{N}\gamma\_k}$$

$$ \pi\_k = \frac{\sum\_{i = 1}^{N} \gamma\_k}{N}$$

\\(\Sigma\_k\\)可以用相似的方法来计算，这里需要注意的是在计算\\( \pi\_k\\)最优值的时候，需要用拉格朗日来解等式约束。

## Spectral Clustering && N-cut

In most cases, the distribution of a mixed dataset can be more complicated than simply clustering around a few cluster centers. In this case, the K-means and mixtures of Gaussians may not group the data correctly. The following figure gives an example where the K-means and mixtures of Gaussians may not group the data correctly. 

<div  align="center">    
<img src="/img/spectralClustering.jpg" width = "500" height = "300"align=center />
</div>

一种解决上述问题的方法是将数据点经过某个合适的非线性变换 (e.g., Laplacian Eigenmaps) ，转换到合适的形式 (例如上面图中的例子)。 Spectral clustering和 Normalized cut (N-cut)算法就是应用了这种思想。关于这两个算法的细节这里不再详细列出，有兴趣的读者可以去看马毅老师的新书[GPCA by Yi Ma]((http://yima.csl.illinois.edu/))。

## Hierarchical Clustering (层次聚类)

层次聚类试图在不同层次上来对数据进行划分，进而形成树形的聚类结构。它对数据的划分是一个“自底向上”的过程。该算法的思想也比较简单：

1. 数据集中的每一个样本为一个初始簇
2. 将距离最近的两个簇合并
3. 不断重复步骤2，直到簇的个数达到指定的个数为止。

距离的衡量方式有以下几种：

1. 两个簇中所有点间距离的最小值
2. 两个簇中所有点间距离的最大值
3. 两个簇的中心点之间的距离
4. 两个簇中所有点间距离的平均值

## Other Methods

### Density-based Clustering

密度聚类是通过样本数据分布的紧密程度来划分数据的，在这里我们主要介绍DBSCAN算法：

1. DBSCAN算法先找出数据集中各样本的\\(\epsilon\\)-邻域, 并确定核心对象集合\\(\Omega\\)
2. 从核心对象集合\\(\Omega\\)中选取一个核心对象作为种子，找出由它密度可达的所有样本，这就构成了一个聚类簇。
3. 然后从\\(\Omega\\)集合中去除步骤2中的核心对象
4. 重复步骤2和3直到\\(\Omega\\)为空。

这个算法中涉及到的相关概念可以参考周志华老师[机器学习][url1]这本书。

### Learning Vector Quantization (LVQ)

LVQ和其他聚类方法不同，LVQ假设数据有类别标记，并利用样本的类别标记信息来辅助聚类：

1. LVQ的目标是学得一组n维原型向量\\(\\{p\_1, p\_2,...p\_q \\} \\), 每个原型向量用来代表一个聚类簇，簇标记\\(t\_i \in \Lambda\\)
2. 初始化原型变量，并为各个原型变量预设标记\\(t\_i\\)
3. 从数据集中随机选取一样本，并找到和该点距离最近的原型向量。
4. 如果该样本点和原型向量的类别标记相同，则使该原型向量靠近该样本点一些，否则远离该样本点一些。
5. 重复步骤3和4直到收敛。

有了学好的一组原型向量\\(\\{p\_1, p\_2,...p\_q \\} \\)，在做inference的时候，我们通过找距离test样本距离最近的原型向量，来得到该test样本点所属的簇。LVQ通常用来改变类别标记的颗粒度大小，例如，如果想使类别标记的更细一些，则可以使原型向量的个数大于原来类别标记种类的数量。

更多细节可以参考周志华老师新书：[机器学习][url1]。

## Applications

聚类有很多的应用场景，例如在图像压缩中，通过存储图像聚类后的中心点来进行图像压缩；聚类也常用来进行异常检测 (离群点检测)，可将远离所有簇中心的样本作为异常点，或将密度极低处的样本作为异常点。

## Reference

- Book: [Pattern Recognition and Machine Learning by Christopher Bishop](prml)
- [Notes of machine learning by Andrew Ng in cousera](http://www.holehouse.org/mlclass)
- [机器学习][url1] 周志华
- Slides by [Prof. Wang](http://sist.shanghaitech.edu.cn/StaffDetail.asp?id=334)
- [统计学习方法](http://www.hangli-hl.com/books.html)
- [GPCA by Yi Ma]((http://yima.csl.illinois.edu/))

[url1]:http://union.click.jd.com/jdc?d=sp9Y9L
[prml]:http://www.rmki.kfki.hu/~banmi/elte/Bishop%20-%20Pattern%20Recognition%20and%20Machine%20Learning.pdf

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

--- 