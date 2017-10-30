title: Linear Models for Classification
date: 2016-04-24 00:10:41
tags: [Machine Learning,Logistic Regression, Softmax]
categories: [Machine Learning, Logistic Regression, Softmax]
description: 本文介绍了Logistic Regression 和用于多分类任务上的softmax。
---
上一篇写了linear models for regression, 这一篇打算写linear models for classification. 当 \\(y\_i\\) 为连续变量时，通过输入 \\(\mathbf{x\_i}\\) 预测 \\(y\_i\\) 为回归(regression)问题; 当 \\(y\_i\\) 为离散变量时，通过输入 \\(\mathbf{x\_i}\\) 预测 \\(y\_i\\) 为分类 (classification) 问题。

# Logistic Regression
Logistic regression 是一种常用的分类方法。对于二分类问题，如果要用一个线性分类器去分类的话，最简单的办法就是对输入\\(\mathbf{x}\\) 做线性组合\\(\theta^T \mathbf{x}\\), 然后将\\(\theta^T \mathbf{x}\\)和阈值\\(\tau\\)进行比较。如果大于该阈值，则为正例；否则为负例。这种和阈值进行比较的方法等效于分段函数。分段函数虽然简单，但它不连续，非凸。

Sigmoid (logistic) 函数可以作为该分段函数的替代。
$$f(\theta^T\mathbf{x}) = \frac{1}{1 + e^{- \theta^T\mathbf{x}}}$$

这样，当 \\(f(\theta^T\mathbf{x}) > \frac{1}{2}\\)时，我们认为\\(\mathbf{x}\\)属于正类；当 \\(f(\theta^T\mathbf{x}) < \frac{1}{2}\\)时，我们认为\\(\mathbf{x}\\)属于负类。我们把用sigmoid函数的这种方法称为 logistic regression。

## 关于Logistic Regression的由来:

我们对 \\(f(\theta^T\mathbf{x}) = \frac{1}{1 + e^{- \theta^T\mathbf{x}}} \\) 进行变换可以得到：\\(\log \frac{f(\theta^T\mathbf{x})}{1-f(\theta^T\mathbf{x})} = \theta^T\mathbf{x}\\). 其中，\\( \frac{f(\theta^T\mathbf{x})}{1-f(\theta^T\mathbf{x})}\\) 称为几率(odd ratio); \\(\log \frac{f(\theta^T\mathbf{x})}{1-f(\theta^T\mathbf{x})}\\)称为对数几率 (log odds,or logit)。 可以看到，\\(\log \frac{f(\theta^T\mathbf{x})}{1-f(\theta^T\mathbf{x})} = \theta^T\mathbf{x}\\) 实际上是用输入的线性组合\\(\theta^T\mathbf{x}\\)来预测对数几率 \\(\log \frac{f(\theta^T\mathbf{x})}{1-f(\theta^T\mathbf{x})}\\)，所以把这种方法称为对数几率回归 Logit Regression (Logistic Regression).

## Cost Function
关于Logistic Regression的cost function，最先想到的应该是MSE，但是我们通常所见到的cost function都是由MLE的方式推出来的。一个不用MSE的原因是：当\\(\theta^T\mathbf{x}\\)接近\\(\frac{1}{2}\\)时，\\(y\_i - f(\theta^T\mathbf{x\_i}) \\)的差值会很大，使得用MSE得到的cost function对actual loss(error rate)的近似效果比较差。([More](http://stackoverflow.com/questions/12157881/cost-function-for-logistic-regression)).

那么不用MSE的话，怎么来定cost function呢？ 可以发现\\(f(\theta^T\mathbf{x})\\)的取值在\\(0 \sim 1\\), 我们可以令
$$f(\theta^T\mathbf{x}) = p(y = 1|\mathbf{x} )\\\\ 1-f(\theta^T\mathbf{x}) = p(y = 0|\mathbf{x})$$
这里\\(y \in \\{0,1\\}\\), 则可以得到 $$p(y|\mathbf{x}) = f(\theta^T\mathbf{x})^{y}(1-f(\theta^T\mathbf{x}))^{1-y}$$

相应的 Likelihood 为：
$$ L = \prod\_{i = 1}^{N} p(y\_i|\mathbf{x\_i})$$

通过取负对数并展开，得到cost function为：
$$E\_{in} = - \frac{1}{N}\sum\_{i = 1}^{N} y\_i \log f(\theta^T\mathbf{x\_i}) + (1 - y\_i)\log (1-f(\theta^T\mathbf{x\_i}))$$

这里对参数\\(\theta\\) 的学习，我们可以使用梯度下降法来得到：
$$\nabla E\_{in} = -\frac{1}{N} \sum\_{i=1}^{N}(y\_i - f(\theta^T\mathbf{x\_i}))\mathbf{x\_i} \\\\ \theta := \theta - \alpha \nabla E\_{in}$$

## 正则化：
为了防止过拟合，通常的做法是在优化目标中加入惩罚项，通过惩罚过大的参数来防止过拟合。常用的正则化有：L1范数和L2范数。L1正则化倾向于使参数变为0，所以可以得到更稀疏的解。 增加L1范数的优化目标：

$$E\_{in} = - \frac{1}{N}\sum\_{i = 1}^{N} y\_i \log f(\theta^T\mathbf{x\_i}) + (1 - y\_i)\log (1-f(\theta^T\mathbf{x\_i})) ＋ \lambda ||\theta||\_1$$

需要注意的是\\(||\theta||\_1\\)不是在所有点处都是可导的，所以这里用不了通常的gradient descent方法，这里可以用Proximal Gradient Descent (PGD) 方法来求解。 关于PGD的方法暂时不在这里介绍，会在后面关于sparse的文章里面进行介绍。

# 多分类：

当y不是在 \\(\\{0,1\\}\\)中取值，而是可能从K个类别中取值时，我们就得到了多(K)分类问题。对于多分类任务，我们可以通过“拆解法”来解决，即将多分类任务拆分为若干个二分类任务来解决。常用的拆分策略有：一对一(OvO), 一对其余(OvR), 多对多(MvM).(更多有关“拆解法”可以参考[周志华老师新书第三章](url1))。

关于多分类任务，更常见的是用 softmax regression来解决。Softmax regression 可以看作是logistic regression在多分类任务上的推广。关于具体怎么推广的，这里不再介绍，有兴趣的读者可以通过 K-1 个\\(\ln\frac{p(y = i)}{p(y = K)} = \theta\_i^T\mathbf{x}\\)等式来推倒(i = 1,2,...K-1)。

Softmax regression用softmax函数来对概率进行建模，具体形式如下：
$$p(y = i|\mathbf{x},\theta) = \frac{e^{\theta\_i^T\mathbf{x}}}{\sum\_{j = 1}^{K} e^{\theta\_j^T\mathbf{x}}}$$

相应的有：
$$ p(y|\mathbf{x},\theta) = \prod\_{i = 1}^{K} p(y = i|\mathbf{x},\theta)^{\mathbb{1}(y = i)} $$

对应的Likelihood：
$$L = \prod\_{j = 1}^{N}  \prod\_{i = 1}^{K} p(y\_j = i|\mathbf{x\_j},\theta)^{\mathbb{1}(y\_j = i)}  $$

对应的cost function为：
$$E\_{in} = -\frac{1}{N} \sum\_{j = 1}^{N}\sum\_{i = 1}^{K} \mathbb{1}(y\_j = i) \ln  \frac{e^{\theta\_i^T\mathbf{x\_j}}}{\sum\_{l = 1}^{K} e^{\theta\_l^T\mathbf{x\_j}}} $$

这里对参数θ 的学习，我们可以使用梯度下降法来得到：
$$\frac{\partial E\_{in} }{\partial \theta\_i}  = -\frac{1}{N} \sum\_{j =1}^{N} \mathbf{x\_j}(\mathbb{1}(y\_j = i) -  \frac{e^{\theta\_i^T\mathbf{x\_j}}}{\sum\_{j = 1}^{K} e^{\theta\_j^T\mathbf{x\_j}}}) $$

关于对 softmax函数求导：
$$\frac{\partial f(x\_i)}{x\_i} = f(x\_i)(1-f(x\_i)) \\\\ \frac{\partial f(x\_i)}{x\_j} = -f(x\_i)f(x\_j) $$

其中，\\(f(x\_i) = \frac{e^{x\_i}}{\sum\_{i = 1}^{K}e^{x\_i}}\\).

由于softmax regression中参数是冗余的，这样就会使得有多组参数满足最优条件。在实际操作中，通常在优化目标基础上加上weight decay(对参数的L2正则化)来解决这一问题。

当K个类别是互斥的，适合使用softmax regression来做多分类任务；当K个类别不是互斥的，即一个\\(\mathbf{x}\\)可以属于多个类别，这个时候适合使用K次OvR logistic regression来做分类。

# 一个简单的 Logistic Regression 例子：
用minfunc优化包来解决：

```matlab
clc;clear;
D = [0 0 0; 2 2 0;2 0 1; 3 0 1];
X = [ones(size(D,1),1) D(:,1:2)];
y = D(:,3);
w = zeros(3,1);
gNorm = Inf;
k = 1;

%% using minFunc :
addpath minFunc_2012/minFunc/
options = struct;
options.maxIter = 400;
options.Method  = 'lbfgs';
options.useMex = 0;
minFuncOptions.display = 'on';

[paraTheta, cost] = minFunc(@(w) logisticCost(w,X,y),w,options);
```
```matlab
function [cost gradient] = logisticCost(w,X,y)
m = size(y,1);
cost = 0;
for i =1:m
    cost =  cost - (y(i)*log(sigmoid(X(i,:),w)) + (1- y(i))*log(1- sigmoid(X(i,:),w))) ;
end
cost = cost / m;
gradient = X'*(sigmoid(X,w)-y);
end
```


用Gradient Descent来解决：

```matlab
%% gradient descent
g_GD = zeros(100,1);
while(gNorm > 1e-5)     
g = X'*(sigmoid(X,w)-y);
w = w - g;
gNorm = norm(g,2);
g_GD(k) = gNorm;
k = k + 1;
end
```

用牛顿法解决：

```matlab
while(gNorm > 1e-5)    
g = X'*(sigmoid(X,w)-y);
S = diag(sigmoid(X,w).*(1-sigmoid(X,w)));
H = X'*S*X;
w = w - inv(H)*g;
gNorm = norm(g,2);
g_NT(k) = gNorm;
k = k + 1;
end

wrongCnt = prediction(w,X,y);
plot(log(g_NT));
```

用拟牛顿法解决：

```matlab
%% BFGS method:

g = X'*(sigmoid(X,w)-y);
B = eye(3,3);
I = eye(3,3);

while norm(g) > 1e-5
   s = -B * g; 
   w = w + s; 
   g_new = X'*(sigmoid(X,w)-y);
   z = g_new - g;
   g = g_new;       
   B = (I - s * z' ./ (z' * s)) * B * ...
   (I - z * s' ./ (z' * s)) + s * s' ./ (z' * s);
   g_BFGS(k) = norm(g);         
   k = k + 1;
end

wrongCnt = prediction(w,X,y);
plot(log(g_BFGS)); 
```
```matlab
function  y = sigmoid(X,w)
y = 1 ./ (1 + exp(-X*w)); 
end
```
```matlab
function [wcnt] = prediction(w,X,y)
y1 = sigmoid(X,w);
yNew = y1 > 0.5;
wcnt = sum(yNew ~= y);
end
```
# Reference

- Book: [Pattern Recognition and Machine Learning by Christopher Bishop](prml)
- [Notes of machine learning by Andrew Ng in cousera](http://www.holehouse.org/mlclass)
- [机器学习][url1] 周志华
- Slides by [Prof. Wang](http://sist.shanghaitech.edu.cn/StaffDetail.asp?id=334)
- [统计学习方法](http://www.hangli-hl.com/books.html)
- [Learning with sparsity-inducing norms](http://www.di.ens.fr/~fbach/mlss08_fbach.pdf)
- [Proximal gradient method](http://www.eecs.berkeley.edu/~elghaoui/Teaching/EE227A/lecture18.pdf)
- [softmax ufldl](http://ufldl.stanford.edu/wiki/index.php/Softmax_Regression)


[url1]:http://cs.nju.edu.cn/zhouzh/zhouzh.files/publication/MLbook2016.htm
[prml]:http://www.rmki.kfki.hu/~banmi/elte/Bishop%20-%20Pattern%20Recognition%20and%20Machine%20Learning.pdf


<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

--- 
