title:  Statics and Optimization in Machine Learning 
date: 2016-03-31 20:50:49
tags: [Machine Learning, SVD, Jesen's Inequality, Matrix Calculus, Optimization]
categories: Machine Learning
description: 本文介绍了机器学习中常用到的统计和优化的基础知识。
---
Machine Learning 是一门集计算机、统计和优化于一体的学科。这一篇打算写一些机器学习中常用到的统计和优化的基础知识。由于时间有限，英文和中文哪个方便就用哪个写了^_^。

# Jenson's Inequality:
Jenson 不等式是机器学习中经常用到的一个不等式。有时我们在evaluate一个变量函数的期望的时候，可能我们不需要知道这个期望的确切数值，只需要知道这个期望的bound就足够了。这时我们就可以用到Jenson不等式。Jenson不等式在推倒EM算法公式的时候会被经常用到。

令 \\(X\\)为一随机变量，\\(f\\)为关于\\(X\\)的凸函数，则有：
 $$ E(f(X)) \geq f(E(X))$$
 如果 \\(f\\)为凹函数，则有，
 $$E(f(X))   \leq f(E(X))$$
 
# SVD 分解:
 
 SVD分解是机器学习中常用到的一种矩阵分解方法。SVD是PCA算法的主要实现手段，而PCA算法又是机器学习中用的最多的降维的方法。
 
 矩阵 \\(X\\) 的 SVD 分解为 \\(X ＝ U\_X \Sigma\_X V\_X^T\\)，其中 \\(X=[x\_1,x\_2,...x\_N] \in R^{D\ \times N}\\)，\\(U\_X \in R^{D\ \times D}\\), \\(\Sigma\_X \in R^{D\ \times N}\\), \\(V\_X \in R^{N\ \times N}\\). 其中，\\(U\_X\\), \\(V\_X\\)为unitary matrix (\\(U U^{\* } = U^{\ \* } U = I \\))，\\(\Sigma\_X\\)为奇异值矩阵。
 
 **PCA via SVD:** 令 \\(X=[x\_1,x\_2,...x\_N] \in R^{D \times N}\\)为减去均值后的数据集，\\(X\\)的SVD分解结果为 \\(X ＝ U\_X \Sigma\_X V\_X^T\\), 则经过降维后的数据集为\\(\Sigma\_X V\_X^T\\)的顶部\\(d\times N\\)部分；降维转换矩阵\\(U\\)为\\(U\_X\\)的前d列。
 
 ps:关于PCA的推倒和实现会在后续文章里提到。暂时打算将降维和维度诅咒写在一起。
 
# Matrix Calculus:
 
 梯度下降法是机器学习中用的最多的算法之一，而它的关键就是对矩阵求导。求导在 deep learning 里面也是关键。然而矩阵求导一直是我的弱项，复杂一点的有时候要求好久，下面好好梳理下矩阵求导。
 
 - 若 \\(\mathbf{x}\\) 为 \\(n\\) 维向量，\\(f：R^{n} \rightarrow R^{m}  \\) 为关于 \\(\mathbf{x}\\) 的函数，则 \\(\(\frac{\partial f}{\partial \mathbf{x}}\)_{ij} = \frac{\partial f\_i }{\partial x\_j}\\). 
 
- 若 \\(\mathbf{x}\\) 为 \\(n\\) 维列向量，\\(f：R^{n} \rightarrow R \\) 为关于 \\(\mathbf{x}\\) 的函数，则 \\(\frac{\partial f}{\partial \mathbf{x}}\\) 为\\(n\\) 维行向量，且\\(\frac{\partial f}{\partial \mathbf{x}} = [\frac{\partial f}{\partial x_{i}}]\\). 
 
- 若 \\(\mathbf{X}\\) 为 \\(m  \times n\\) 的矩阵， \\(f：R^{m\ \times n} \rightarrow R \\) 为关于 \\(\mathbf{X}\\) 的函数，则 \\(\frac{\partial f}{\partial \mathbf{X}}\\) 为\\(n \times m\\) 的矩阵，且\\([\frac{\partial{f}}{\partial \mathbf{X}}]\_{ij} = [\frac{\partial f}{X\_{ji}}]\\).
 
**矩阵求导的三步：**
 
- 1  弄清求导的维度
- 2  根据求导的维度，将 \\(f\\) 展开成关于 \\(x\_i\\) 的形式，然后用element-wise calculations 算出每一项的导数
- 3  将算出来的每一项导数放到向量或者矩阵里面。
 
## Example 1: scalar by vector 
 \\( \frac{\partial (\mathbf{x}^{T}\mathbf{a})}{\partial \mathbf{x}} =  \frac{\partial (\mathbf{a}^{T} \mathbf{x})}{\partial \mathbf{x}} = \mathbf{a^{T}}\\) .
 
 根据矩阵求导三步：1. \\(\mathbf{x}^{T}\mathbf{a}\\) 是两个向量的内机，为scalar；\\( \mathbf{x}\\)是向量，所以 \\(\frac{\partial f}{\partial \mathbf{x}} = [\frac{\partial f}{\partial x\_{i}}]\\) 是一个行向量. 2. \\(\mathbf{x}^{T} \mathbf{a} ＝ \sum\_{i=1}^{n} a\_i x\_i\\), 所以 \\(\frac{\partial f}{\partial x\_{i}} = a\_i\\). 3. 将每一项求导结果放在向量中就得到了结果：\\( \mathbf{a^{T}}\\).
 
## Example 2: 
 \\( q(x) = || Ax - b||\_2^2\\), where \\(A\\) is \\(m \times n\\), \\(x\\) is \\(n \times  1\\), \\(b\\) is \\(m \times 1\\), \\(\frac{\partial q(x)}{x} =  ?\\)
 
 首先看几个其他公式：
 
 - \\(y = Ax\\), \\(\frac{\partial y}{\partial x} = A\\), \\(\frac{\partial y}{\partial z} = A \frac{\partial x}{\partial z}\\)
 - \\(\alpha = y^T Ax\\), \\(\frac{\partial \alpha}{\partial x} = y^T A\\); \\(\alpha = x^T A^T y\\),\\(\frac{\partial \alpha}{\partial y} = x^T A^T\\); \\( \frac{\partial \alpha}{\partial A} = yx^{T}\\);
 - \\(\alpha = y^T x\\), \\(\frac{\partial \alpha}{\partial z} = x^T \frac{\partial y}{\partial z} + y^T \frac{\partial x}{\partial z}\\); Proof: \\(\frac{\partial \alpha}{\partial z} = \frac{\partial \alpha}{\partial y}\frac{\partial y}{\partial z}+ \frac{\partial \alpha }{\partial x}\frac{\partial x}{\partial z} = x^T \frac{\partial y}{\partial z} + y^T \frac{\partial x}{\partial z} \\)
 - \\(\alpha = x^Tx\\),\\(\frac{\partial \alpha}{\partial x} = 2x^T\\); \\(\alpha = x^TAx\\), \\(\frac{\partial \alpha}{\partial x} = (Ax)^T + x^TA = x^T(A^T + A)\\)
 
回到我们这个问题上，\\( q(x) = || Ax - b||\_2^2 ＝ (Ax-b)^T (Ax-b)\\), 则从上面公式可得：\\(\frac{\partial q(x)}{\partial x} = 2(Ax-b)^T \frac{\partial (Ax-b)}{\partial x} = 2(Ax-b)^TA\\).
 
 \\(f(x)\\) 关于\\(x\\) 的二阶导数称为Hessian matrix：\\([\frac{\partial^2f(x)}{\partial x^2}]_{ij} = \frac{\partial^2f(x)}{\partial x\_i \partial x\_j}\\), 其中，\\(\mathbf{x}\\) 为 \\(n\\) 维列向量，\\(f：R^{n} \rightarrow R \\) 为关于 \\(\mathbf{x}\\) 的函数。
 
 ps：更多关于矩阵求导相关公式会在深度学习中提到。
 
#  Optimization:
 
 刚看了周志华老师[新书](url1)的附录部分，发现我想写的里面都有，而且深入浅出，浅显易懂^_^。关于优化部分，主要想写下拉格朗日乘子法、KKT条件和一些其他的凸优化相关知识。
 
## 拉格朗日乘子法：
 拉格朗日乘子法是寻找多元函数在一组约束条件下极值的方法；通过引入拉格朗日乘子，将d个变量在k个约束条件下的极值问题转化成d＋k个变量无约束的极值问题。[PRML](prml)这本书的附录E里面有更加详细的关于拉格朗日乘子的介绍。
 
### 等式约束：
 假设 \\(\mathbf{x}\\) 为 d 维向量，我们要找到一个取值\\(\mathbf{x^{\*}\}\\), 使得目标函数 \\(f(\mathbf{x})\\) 在约束条件\\(g(\mathbf{x})=0\\) 下取得最小值。 满足约束条件\\(g(\mathbf{x})=0\\) 的点在一个 \\(d-1\\) 维的曲面上，对于约束曲面上的任意一点\\(x_i\\)，它的梯度 \\(\nabla g\(\mathbf{x\_i}\)\\) 都正交于该曲面。对于我们要找的 \\(\mathbf{x^{\*}}\\), 目标函数在该点的梯度 \\(\nabla f\(\mathbf{x^\*}\)\\) 也要正交于约束曲面。因为如果不正交的话，我们可沿着梯度方向在约束曲面上移动一小段距离，从而可以找到更小的值。也就是\\(\nabla g\(\mathbf{x}\) + \lambda \nabla f\(\mathbf{x}\) = 0\\), 其中，\\(\lambda \neq 0\\)。
 
 下面引出拉格朗日函数：\\(L(\mathbf{x},\lambda) = f(\mathbf{x}) + \lambda g(\mathbf{x})\\). 将 \\(L(\mathbf{x},\lambda)\\) 对 \\(\mathbf{x}\\) 求导可得：\\(\nabla f\(\mathbf{x}\) + \lambda \nabla g\(\mathbf{x}\) = 0\\)； 对\\(\lambda \\) 求导可得：\\(g(\mathbf{x})=0\\). 所以对拉格朗日函数的无约束优化即等价于原等式约束优化。
 
### 不等式约束：
当\\(g(\mathbf{x}) \leq 0\\), 有两种情况：

-  \\(g(\mathbf{x}) < 0\\) 时, 约束条件 \\(g(\mathbf{x}) \leq 0\\) 不起作用，此时等价于 \\(\lambda ＝ 0\\)，可直接通过 \\(\nabla f(\mathbf{x}) = 0\\) 求解.
-  \\(g(\mathbf{x}) = 0\\) 时，情况和上一条等价约束比较类似，不过此处 \\( \lambda> 0\\).

由此引出**KKT条件：** 

- \\(\nabla g\(\mathbf{x}\) + \lambda \nabla f\(\mathbf{x}\) = 0\\)
- \\(g(\mathbf{x}) \leq 0\\)
- \\( \lambda \geq 0\\)
- \\( \lambda g(\mathbf{x}) = 0 \\)

综上，当有多个约束时，考虑有 m 个等式约束和 n 个不等式约束,
$$\min\_{\mathbf{x}} f(\mathbf{x}) \\\\ s.t. \\ h\_{i}(\mathbf{x})=0 \\ (i = 1,2,...m) \\\ g\_{j}(\mathbf{x}) \leq 0 \\ (j = 1,2,...n) $$

引入拉格朗日乘子，\\(\mathbf{\lambda} = \(\lambda\_{1},\lambda\_{2},\ldots,\lambda\_{m}\)^{T}\\), \\(\mathbf{\mu} = \(\mu\_{1}, \mu\_{2},\ldots,\mu\_{n}\)^{T} \\), 得到拉格朗日函数为：

\\(L\(\mathbf{x},\mathbf{\lambda},\mathbf{\mu}\) = f(\mathbf{x}) + \sum\_{i=1}^{m}\lambda\_{i} h\_{i}(\mathbf{x}) + \sum\_{j=1}^{n} \mu\_{j} g\_{j}(\mathbf{x})\\)

其中，由不等式引入的 KKT 条件为 (\\(j = 1,2,\ldots,n\\))：

- \\( g\_{j}(\mathbf{x}) \leq 0  \\)
- \\(\mu_j \geq 0\\)
- \\(\mu\_j g\_j(\mathbf{x}) = 0\\)


## Convexity Optimization
通过上面拉格朗日乘子和KKT条件算出的结果只是必要条件，只有当目标函数是凸的情况下，才能保证是充分必要条件 (取到全局最优)。通常来讲，找一个函数的全局最优解是很难的；但是如果我们可以将一个问题formulate 成凸优化问题，解决起来会容易很多。

### Convex Sets
在判断一个函数是不是凸函数的时候，首先要看它的定义域是不是一个凸集；所以首先介绍下什么是凸集。

**Definition:** A set \\(C \\) is convex, if for all \\(x,y \ in \ C \\) and \\(\theta \in R\\) with \\(0 \leq \theta \leq 1\\), \\(\theta x + (1-\theta)y \in C \\). 意思就是说，对于集合中任意两个元素，我们在这两个元素之间做一条线段，如果线段上的所有点都在集合内，则这个集合就是凸的。

常见的凸集有：

- All of \\(R^{n}\\)
- The non-negative orthant, \\(R\_{+}^{n}\\): \\(R\_{+}^{n} = \\{x: x\_i \geq 0 \ \forall i= 1,2,...n \\}\\)
- Norm balls: e.g., \\(||x|| \leq 1\\)
- Affine subspaces \\(\\{x \in R^{n} : Ax = b\\}\\) and polyhedra \\(\\{x \in R^{n} : Ax \preceq b\\}\\), where \\(\preceq \\) denotes componentwise inequality.
- Intersections of convex sets
- Positive semidefine matrices \\(S\_{+}^{n}\\): \\(A = A^{T},\\) and for all \\(x \in R^{n}\\), \\(x^{T}Ax \geq 0\\).

### Convex Functions:

凸函数是凸优化中一个比较核心的概念，被用在凸优化各个方面。

**Definition:** A function \\(R^{n} \rightarrow R \\) is convex, if its domain (denoted as D(f)) is a convex set, and if for all \\(x,y \in D(f)\\) and \\(\theta \in R \\) with \\(0 \leq \theta \leq 1\\), \\(f(\theta x + (1-\theta)y) \leq \theta f(x) + (1-\theta)f(y)\\). 

如果该等式左侧在该条件下(且\\(x \neq y\\))恒小于右侧，我们就说该函数是 strictly convex。 如果一个函数 \\(f \\)的负数 \\(-f \\) 是凸函数，则该函数 \\(f\\)为凹函数。

如果函数 \\(f \\) 是凸函数，则它的局部最小值即为它的全局最小值。如果 \\(f \\) 是strictly convex的，\\(f \\) 最多只有一个全局最小值；也就是说如果\\(f \\)有最小值，那么这个最小值就是唯一的。

**First Order Condition of Convexity:**
Suppose a function \\(R^{n} \rightarrow R \\) is differentiable (i.e., the gradient \\(\nabla f(x)\\) exists for all \\(x\\) in the domain of \\(f\\)). Then \\(f \\) is convex, if and only if \\(D(f)\\) is a convex set and for all \\(x, y \in D(f)\\), \\(f(y) \geq f(x) + \nabla f(x)^{T}(y-x)\\). 也就是说我们在点\\(x\\)处做切线，用这个切线上的点来近似函数\\(f\\)上的点，那么这个切线上的所有点都在函数\\(f\\)相应点的下方。如果恒大于的话，即为strictly convex；如果不等号相反，那么\\(f\\)即为凹函数。

**Second Order Condition of Convexity:**
 Suppose a function \\(R^{n} \rightarrow R \\) is twice differentiable (i.e., the Hessian Matrix \\(\nabla^{2} f(x)\\) is defined for all \\(x\\) in the domain of \\(f\\)). Then \\(f \\) is convex, if and only if \\(D(f)\\) is a convex set and \\(\nabla^{2} f(x)\\) is positive semidefine. If \\(\nabla^{2} f(x)\\) is positive define, then \\(f\\) is strictly convex. If \\(\nabla^{2} f(x)\\) is negative semidefine, then \\(f\\) is concave. 当\\(x\\)为一维的时候，即为\\(\nabla^{2} f(x) \geq 0\\)

If \\(f\\) is convex, then the \\(\alpha\\)-sublevel set (\\(x \in D(f): f(x) \leq \alpha\\)) is a convex set.


**常见的凸函数**

- Exponential functions
- Negative logarithm:
- Affine functions: \\(f(x) = b^{T}x + c\\), \\(b \in R^{n}, \ c \in R\\). In this case \\(\nabla^{2} f(x) = 0\\), the affine functions of this form are the \\(\mathbf{only}\\) functions that are both convex and concave.
- Quadratic functions: \\(f(x) = x^{T}Ax + b^{T}x + c \\), when \\(A\\) is symmetric and \\(A\\) is positive semidefine, \\(f(x)\\) is convex. ( \\(\nabla^{2} f(x) = A\\)).
- Norms (using the definition to prove it)
- Nonnegative weighted sums of convex functions

### Convex Optimization Problems:
$$\min_x f(x) \\\\ s.t. g\_i(x)\leq 0, \ i= 1,2,...n \\\\ h\_j(x) = 0,\ j = 1,2,...n $$
where \\(f\\) is a convex function, \\(g\_i\\) are convex functions and \\(h\_i\\) are affine functions.

**Special cases of convex problems:**

- Linear Programming (LP):
$$\min\_x c^{T}x + d \\\\ s.t. Gx \preceq h \\\\ Ax = b$$
where \\(\preceq \\) denotes elementwise inequality.
- Quadratic Programming (QP):
$$ \min\_x \frac{1}{2} x^{T}Px + c^{T}x + d \\\\ s.t. Gx \preceq h \\\\ Ax = b$$ where \\(P\\) is a symmetric positive semidefine matrix.
- Quadratically Constrained Quadratic Programming (QCQP):
$$\min\_x \frac{1}{2} x^{T}Px + c^{T} +d \\\\ s.t. \frac{1}{2} x^{T}Q\_ix + r\_i^{T}x + s\_i \leq 0 \ \ i = 1,2...,m \\\\ Ax= b$$
where \\(P\\) and \\(Q\_i\\) are symmetric positive semidefine matrices. 

# 共轭分布、多项分布与 Dirichlet 分布
在很多模型中 (e.g., LDA)，一个节点到其他节点的概率之和为1，这时我们就用多项分布来建模。比如现在我们有一个\\(d\\)维向量\\(\mathbf{x}\\), 其中\\(\mathbf{x}\\)是 1-of-K 的形式。令\\(x\_i\\)取1 的概率为\\(\mu\_i\\), 并且\\(\sum\_{i = 1}^{d}\mu\_i = 1\\),那么我们就能得到关于\\(\mathbf{x}\\)的多项分布：\\(P(\mathbf{x}|\mathbf{\mu}) = \prod\_{i=1}^{d} \mu\_i^{x\_i}\\). 其中，\\(\mathbf{\mu}\\)即为我们模型的参数。

现在我们想对参数\\(\mathbf{\mu}\\)加先验，我们认为参数\\(\mathbf{\mu}\\)服从一个分布。通常来讲，我们都会选共轭分布来作为先验分布 (在很多情况下，共轭分布能使问题简化)。多项分布的共轭分布是Dirichlet 分布，因为将多项分布和Dirichlet 分布相乘仍是Dirichlet 分布。在生成模型中，多项分布和Dirichlet 分布用的比较多，这里先简单介绍下，在后面的LDA模型中，我会详细介绍。







# Reference

- Book: [Pattern Recognition and Machine Learning by Christopher Bishop](prml)
- [notes of machine learning by Andrew Ng in cousera](http://www.holehouse.org/mlclass)
- GPCA (Generalized Principle Component Analysis) by [Yi Ma](http://yima.csl.illinois.edu/) (to be published)
- [机器学习][url1] 周志华
- [Matrix Differentiation](MD)
- [Matrix CookBook](MCB)
- [Notes of CS229 in Stanford](http://cs229.stanford.edu/)



[url1]:http://cs.nju.edu.cn/zhouzh/zhouzh.files/publication/MLbook2016.htm
[prml]:http://www.rmki.kfki.hu/~banmi/elte/Bishop%20-%20Pattern%20Recognition%20and%20Machine%20Learning.pdf
[MD]:http://www.atmos.washington.edu/~dennis/MatrixCalculus.pdf
[MCB]:https://www.math.uwaterloo.ca/~hwolkowi/matrixcookbook.pdf

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

