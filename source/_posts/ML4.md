title: Linear Models for Regression
date: 2016-04-12 21:17:18
tags: [Machine Learning, Linear Regression, Gradient Descent, Newton Method, Ridge Regression, Lasso] 
categories: Machine Learning
description: 本文介绍了Linear Regression 和常用的正则化方法以及常用的优化方法。
---
线性模型：模型输出对模型参数成线性。线性模型可以看作是一组输入变量非线性映射的线性组合。线性模型比较简单，通常在介绍一些较复杂的模型之前，先介绍线性模型作为基础；线性模型也常用作解释机器学习中一些现象的例子 (e.g., curse of dimensionality)。

令 \\( D = \\{(\mathbf{x\_1},y\_1),(\mathbf{x\_2},y\_2)...,(\mathbf{x\_m},y\_m)\\}\\) 为数据集，其中 \\(\mathbf{x} = [x\_1;x\_2;...;x\_n]\\) 为n维列向量。当 \\(y\_i\\) 为连续变量时，通过输入 \\(\mathbf{x\_i}\\) 预测 \\(y\_i\\) 为回归(regression)问题; 当 \\(y\_i\\) 为离散变量时，通过输入 \\(\mathbf{x\_i}\\) 预测 \\(y\_i\\) 为分类 (classification) 问题。

# Linear Regression 
线性回归(linear regression) 试图学到：
$$ f(\mathbf{x}) = \theta\_0 + \theta\_1 x\_1 + \theta\_2 x\_2 + ... + \theta\_n x\_n$$
用矩阵的形式表示为： 
$$f(\mathbf{x}) = \mathbf{x}^T \mathbf{\theta} $$
其中，\\(\mathbf{\theta} = [\theta\_0;\theta\_1;\theta\_2;...;\theta\_n]\\); \\(\mathbf{x} = [1,\mathbf{x}^T]\\);  \\(f(\mathbf{x})\\)为\\(\mathbf{x}\\)的预测值。

线性回归的cost function为均方误差MSE：
$$J(\theta) = \frac{1}{2} (\mathbf{X} \mathbf{\theta}  - \mathbf{y})^T( \mathbf{X} \mathbf{\theta} - \mathbf{y}) = \frac{1}{2}||\mathbf{X} \mathbf{\theta}  - \mathbf{y}||\_2^2$$
其中，\\(\mathbf{X} = [\mathbf{x\_1}; \mathbf{x\_2};... \mathbf{x\_m}]\\), \\(\mathbf{x\_i} 为加入1之后的向量\\)。

我们将基于均方误差最小化来求解模型的方法称为“最小二乘法” (least square method).

线性回归的cost function为凸函数，我们可以直接对其求导来得到最优的\\(\theta\\)。
$$ \frac{\partial J(\theta)}{\partial \theta} = \mathbf{X}^T(\mathbf{X}\theta - \mathbf{y})$$
从而得到 \\(\theta^* = (\mathbf{X}^T\mathbf{X})^{-1}\mathbf{X}^T\mathbf{y}\\), 其中 \\((\mathbf{X}^T\mathbf{X})^{-1}\\) 为矩阵\\(\mathbf{X}^T\mathbf{X}\\)的逆矩阵。

然而，当数据\\(\mathbf{x}\\) 的维度很高时，\\((\mathbf{X}^T\mathbf{X})^{-1}\\) 求解需要很长时间。更常用的方法是梯度下降法 (gradient descent).

# Gradient Descent
函数值沿着负梯度方向下降最快，梯度下降法就是基于这一点的：

$$\theta := \theta - \alpha \nabla J(\theta)$$

其中\\(\alpha\\)为学习率(learning rate)，用来控制每一步沿着负梯度方向走的步伐的大小。\\(\alpha\\)既不可太大，也不可太小。当\\(\alpha\\)太小时，要迭代很多次，运行时间过长；当\\(\alpha\\)太大时，可能会发生震荡，找不到(局部)最优值。梯度下降法是常用的一阶优化方法，但是需要对学习率进行调参。

根据计算梯度时所用的数据数量，gradient descent有三种变化：

- **Batch Gradient Descent**: $$\theta := \theta - \alpha \nabla J(\theta)$$ Batch gradient descent使用全部数据计算梯度, 所以更新一次参数\\(\theta\\)需要很长的时间。并且，batch gradient descent不能进行online learning。Batch gradient descent 能够保证收敛到全局最小值(目标函数为凸函数)或者局部最小值(目标函数为非凸函数)。
- **Stachastic Gradient Descent (SGD):** 
$$\theta := \theta - \alpha \nabla J(\theta;\mathbf{x}\_i,y\_i)$$ 
Stachastic gradient descent 每次使用一个数据去更新参数，使得参数更新很快，并且SGD可用作online learning。由于SGD更新的太过频繁，引入了过高的variance，使得目标函数\\(J(\theta)\\)波动较大 ([见图](https://en.wikipedia.org/wiki/Stochastic_gradient_descent))。
- **Mini-batch Gradient Descent:** $$\theta := \theta - \alpha \nabla J(\theta;\mathbf{x}\_{i:i+n},y\_{i+n})$$ 
Mini-batch gradient descent 使用n个数据更新参数，减小了SGD更新参数时的variance，达到更稳定的收敛。Mini-batch gradient descent是训练神经网络的一个最常用选择。

关于更多的gradient descent optimization algorithm (momenterm,Adagrad...) 暂时不在这篇博客里写了，感兴趣的可以先看下这一篇 [An overview of gradient descent optimization algorithms ](http://sebastianruder.com/optimizing-gradient-descent/).

# Newton Method
当目标函数二阶连续可微时，可以使用牛顿法。牛顿法的迭代次数远小于梯度下降法。

在高数中，牛顿法是求解方程的一种近似迭代解法。例如在求解方程 \\(f(x) = 0\\) 时，当用求根公式求不出的时候，可以用牛顿法。对 \\(f(x)\\) 在 \\(x\_0\\) 处泰勒展开得到： \\(f(x) = f(x\_0) + (x - x\_0) \nabla f(x\_0)\\), 进而有 \\(x = x\_0 - \frac{f(x\_0)}{\nabla f(x\_0)}\\)。 我们可以根据  \\(x = x\_0 - \frac{f(x\_0)}{\nabla f(x\_0)}\\) 进行迭代来求出方程的解。

在优化问题中，我们想知道的是方程 \\(\nabla f(x) = 0\\) 的解。当函数二阶连续可微时，我们可以得到方程 \\(\nabla f(x) = 0\\) 的迭代公式： \\(x = x\_0 - \frac{\nabla f(x\_0)}{\nabla^2 f(x\_0)}\\)。应用在线性回归问题上即为：
$$ \theta := \theta - \frac{\nabla J(\theta)}{\nabla^2 J(\theta)} := \theta - H(J(\theta))^{-1}\nabla J(\theta)$$

其中，\\(H(J(\theta))^{-1}\\)为海森矩阵的逆矩阵，计算复杂度相当高，在高维问题中几乎不可行。拟牛顿法用较低的计算代价来寻找海森矩阵逆矩阵的近似矩阵，从而可显著性的降低计算开销。比较常用的拟牛顿法是l-bfgs算法。[minFunc](https://www.cs.ubc.ca/~schmidtm/Software/minFunc.html) 是机器学习中一个常用的matlab优化包, 只需要自己实现cost和梯度，用minFunc实现线性回归的代码如下：

```matlab
addpath minFunc_2012/minFunc/
x = 1:1:12;
x = x';
y = [0.74 1.41 1.45 1.49 1.21 1.31 1.68 2.04 1.58 1.23 1.50 1.67 ]*0.01;
y = y';

%% using minFunc :
options = struct;
options.maxIter = 400;
options.Method  = 'lbfgs';
options.useMex = 0;
minFuncOptions.display = 'on';
theta0 = [0; 0];
X = [ones(size(x)) x];
[paraTheta, cost] = minFunc(@(p) linear_regression(p,X,y),theta0,options);
yNew = X*paraTheta;

```
```matlab
function [cost, gradient] = linear_regression(theta,X,y)
 cost = 0.5*norm(X*theta-y,2);
 gradient = X'*(X*theta - y);
end
```
# Linear Basis Function Models
对于一般的线性模型有：
$$ f(\mathbf{x}) = \theta\_0 + \sum\_{j = 1}^{M-1} \theta\_j \phi\_j(\mathbf{x}) = \phi(\mathbf{x})\theta $$

其中，\\(\phi(\mathbf{x})\\) 为基函数，常用的基函数有：多项式函数、高斯函数、sigmoid函数。相应的cost function为：

$$J(\theta) = \frac{1}{2} ||\Phi \theta - \mathbf{y}||\_2^2$$

相应的最优解 \\(\theta^{*} = (\Phi^T \Phi)^{-1} \Phi^T \mathbf{y}\\).

# Ridge Regression
以多项式回归为例，当模型变得更复杂 (所用的\\(x^{j}\\)阶数越高)时，会出现overfitting的情况，可以发现此时相应的参数\\(\theta\_i\\)会非常大。通常用来控制这种overfitting的做法是，对参数加一个限制：\\(\theta^T\theta \leq C\\).

这样就得到了如下的 ridge regression:

$$ \min\_{\theta} \ J(\theta) = \frac{1}{2} ||\Phi \theta - \mathbf{y}||\_2^2 \\\\ s.t. \ \theta^T\theta \leq C$$.

上式等价于：
$$ \min\_{\theta} \ J(\theta) = \frac{1}{2} ||\Phi \theta - \mathbf{y}||\_2^2 ＋\frac{\lambda}{2} \theta^T\theta $$.

Ridge regression 是一个典型的凸优化问题，可以通过凸优化包来解决。

# Lasso
当我们用1-范数对参数\\(\theta\\)进行约束时，就得到了LASSO：

$$ \min\_{\theta} \ J(\theta) = \frac{1}{2} ||\Phi \theta - \mathbf{y}||\_2^2 \\\\ s.t. \ ||\theta||\_1 \leq C$$.

上式等价于：
$$ \min\_{\theta} \ J(\theta) = \frac{1}{2} ||\Phi \theta - \mathbf{y}||\_2^2 ＋\lambda ||\theta||\_1 $$.

## Optimality Condition for LASSO:
对 \\(J(\theta) = \frac{1}{2} ||\Phi \theta - \mathbf{y}||\_2^2 ＋\lambda ||\theta||\_1 \\) 求导时，我们发现第一项是可导的，但是第二项在\\(\theta\_i\\)为0时，不可导。这里需要引入次微分和次梯度来解决。次微分和次梯度是对导数的一个推广(不可导的情况)，通常用在凸优化中。

对一个凸函数\\(f\\)有 \\(f(y) \geq f(x) + \nabla f(x)^T(y-x)\\), 则 \\(g\\) is a subgradient of a convex function \\(f\\) at \\(x \in D(f)\\) if : 
$$ f(y) \geq f(x) + g^T (y-x)$$

次微分(subderivative)是所有满足 \\(f(y) \geq f(x) + g^T (y-x)\\) 的\\(g\\) 形成的集合\\(\partial f(x)\\)。对于非光滑凸函数，一阶最优性条件为：\\(0 \in \partial f(x)\\) 或者  \\(\exists g \in \partial f(x), \ s.t. \ g = 0\\)。(即对于光滑可导函数，我们将导数为零作为一阶最优条件；对于非光滑凸函数，我们将0是否在次微分集合作为一阶最优条件)。

回到LASSO这个问题上，我们可以得到其一阶最优化条件：
$$0 \in \Phi^T(\Phi\mathbf{X} - \mathbf{y}) + \lambda \partial ||\theta||\_1$$


# Reference

- Book: [Pattern Recognition and Machine Learning by Christopher Bishop](prml)
- [Notes of machine learning by Andrew Ng in cousera](http://www.holehouse.org/mlclass)
- [机器学习][url1] 周志华
- Slides by [Prof. Wang](http://sist.shanghaitech.edu.cn/StaffDetail.asp?id=334)
- [Sebastian Ruder's blog - An overview of gradient descent optimization algorithms ](http://sebastianruder.com/optimizing-gradient-descent/)
- [统计学习方法](http://www.hangli-hl.com/books.html)
- [subderative](https://en.wikipedia.org/wiki/Subderivative)
- [subgradients by Stephen Boyd](https://see.stanford.edu/materials/lsocoee364b/01-subgradients_notes.pdf)

[url1]:http://cs.nju.edu.cn/zhouzh/zhouzh.files/publication/MLbook2016.htm
[prml]:http://www.rmki.kfki.hu/~banmi/elte/Bishop%20-%20Pattern%20Recognition%20and%20Machine%20Learning.pdf


<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>
