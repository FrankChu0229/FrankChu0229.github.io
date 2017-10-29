title: Support Vector Machines
date: 2016-05-09 22:56:05
tags: [Machine Learning, Support Vector Machines, SVM]
categories: Machine Learning
description: 本文介绍了支持向量机 (Support Vector Machines)的来源、原问题和对偶问题的推倒以及优化，核函数和Soft-margin SVM。
---
# Support Vector Machines

## Introduction
在logistic regression中，分类器只要将两个类别的点分开即可使cost function达到最小，但是哪一种分类器更好呢？下面的三个logistic regression分类器都能将两个类别的点完全分开。从图中我们可以直观的看出，距离分离平面越近的点越容易分类错误，而距离分离平面越远的点则越不容易分类错误。下面三幅图中，图三中的点最不容易分类错误。所以我们想使所有的点分类正确，同时所有的点距离分离平面尽可能的远，可以理解为我们要最大化所有点中到分类平面的最小距离, 这也是SVM的目标。

<div  align="center">    
<img src="/img/logisticRegression.png" width = "500" height = "200" 
align=center />
</div>

我们知道点\\(x\_i\\)到分类平面的几何距离为\\(\frac{|w^Tx\_i+b|}{||w||}\\),而如果该平面能够作为分类平面，则必有：\\(y\_i(w^Tx\_i+b) > 0\\). 我们的目标是最大化所有点到分离平面的最小距离。通过rescaling \\(w,b\\),可以得到: \\(\min_{i = 1,2,...m} y\_i(w^Tx\_i + b) = 1 \\) (**这个地方可能不是很好理解，需要注意的是：1.平面的w和b可以变化到相应的倍数而这个平面仍然保持不变；2.假设当前的w和b使得距离分类平面最近的点的\\(y(w^Tx + b) = D\\), 则我们可以将w和b缩小\\(\frac{1}{D}\\),从而使\\(y(w^Tx + b) = 1\\).**),这样我们可以得到所有点中到分离平面的最小距离为: \\(\frac{1}{||w||}\\), 我们把这个值称为**margin**。由此，我们可以得到SVM的优化目标和限制条件：

$$ \max_{w,b} \ \  \frac{1}{||w||}  \\\\ s.t. \ \ y\_i(w^Tx\_i + b) \geq 1$$
相应地，可以变为，

$$ \min\_{w,b} \ \  \frac{1}{2}||w||_{2}^{2}  \\\\ s.t. \ \ y\_i(w^Tx\_i + b) \geq 1$$

由此，SVM的目标函数就变成了一个标准的凸二次规划问题，有很多开源的优化包都可以用来解这个问题。Here is an example using the active set optimization method in matlab (first you need to transform the equations into vector form):

```matlab
X = [ 3 1; 3 -1; 6 1; 6 -1; 1 0 ; 0 1; 0 -1; -1 0];
y = [1 1 1 1 -1 -1 -1 -1]';

%% plot the data:
plot(X(1:4,1),X(1:4,2),'+')
hold on
plot(X(5:8,1),X(5:8,2),'o')
hold on

%% primal problem:
w = [0 0]';
wb = 0;
theta = [wb;w];
H = [0 0 0; 0 1 0; 0 0 1];
f = [0 0 0]';
b = -1*ones(size(X,1),1);
A = -1*[y, [y,y].*X];
options = optimoptions('quadprog',...
    'Algorithm','active-set','Display','off');

[theta,fval,exitflag,output,lambda] = ...
   quadprog(H,f,A,b,[],[],[],[],[],options);
wb = theta(1);
w = theta(2:end);
X*w + wb
```

## Dual Problem of SVM

更常用的求解SVM的方法是求解其对偶问题。

首先我们可以将优化目标写成拉格朗日函数的形式：
\\(L(w,b,\alpha) = \frac{1}{2}w^Tw + \sum_{i=1}^{m}\alpha\_i(1-y\_i(w^Tx\_i+b))\\), 相应的与约束条件等价的形式为: \\(\max\_{\alpha\_i > 0} L(w,b,\alpha)\\)。那么关于SVM，我们可以表示为：\\(\min\_{w,b} \max\_{\alpha\_i > 0} L(w,b,\alpha)\\). 由于上述表达式属于强对偶的形式，所以原问题的解即为对偶问题的解。由此，我们可以得到SVM的对偶问题为：

$$ \max\_{\alpha} \min\_{w,b} L(w,b,\alpha) ＝  \max\_{\alpha} \min\_{w,b}  \frac{1}{2}w^Tw + \sum_{i=1}^{m}\alpha\_i(1-y\_i(w^Tx\_i+b))$$

首先，求解该拉格朗日函数关于\\(w,b\\)的最小值的解，对\\(L(w,b,\alpha)\\) 对\\(w,b\\)求偏导可得，

$$\frac{\partial L}{\partial w} = w － \sum\_{i=1}^{m} \alpha\_ix\_iy\_i ＝ 0$$

$$\sum\_{i=1}^{m}\alpha\_iy\_i = 0$$

将相应的\\(w,b\\)带入可得对偶问题的优化目标为：

$$\max\_{\alpha} L(w,b,\alpha) = -\frac{1}{2}\sum\_{i=1}^{m}\sum\_{j=1}^{n}\alpha\_i\alpha\_jy\_iy\_jx\_i^Tx\_j + \sum\_{i=1}^{m} \alpha\_i$$

相应的对偶问题的约束条件为：

$$\alpha\_i \geq 0 \\\\ \sum\_{i=1}^{m}\alpha\_iy\_i = 0$$

对偶问题依然是一个凸二次规划问题，可以用常用的优化包来求解。这样在求得最优的\\(\alpha^{\*}\\)之后，通过\\(w^{\*} = \sum\_{i=1}^{m} \alpha^{\*}\_ix\_iy\_i\\)即可求得相应的最优w值。那么相应的b怎么求呢？我们可以看原问题的KKT条件：

$$\alpha\_i \geq 0 \\\\ 1 - y\_i(w^Tx\_i+b) \leq 0 \\\\ \alpha\_i(1 - y\_i(w^Tx\_i+b)) = 0$$

当\\(\alpha\_i > 0\\)时，相应的有\\(y\_i = (w^Tx\_i+b)\\), 此时的(\\(x\_i,y\_i\\))即为support vectors，最优的分类平面即由这些support vectors决定。由这些support vectors，我们可以得到:

$$b^{\*} = y\_s - {w^{\*}}^Tx\_s = y\_s - \sum\_{i=1}^{m} \alpha^{\*}\_iy\_ix\_i^Tx\_s$$

Here is an example using the optimization method in matlab to solve the dual problem of SVM (first you need to transform the equations into vector form):

```matlab
X = [ 3 1; 3 -1; 6 1; 6 -1; 1 0 ; 0 1; 0 -1; -1 0];
y = [1 1 1 1 -1 -1 -1 -1]';
a = zeros(size(X,1),1);
f = -1*ones(size(X,1),1);
lb = zeros(size(X,1),1);
beq = 0;
Aeq = y';
G = ([y y].*X) * ([y y].*X)';

options = optimoptions('quadprog',...
    'Algorithm','interior-point-convex','Display','off');

[a,fval,exitflag,output,lambda] = ...
   quadprog(G,f,[],[],Aeq,beq,lb,Inf,[],options);

t = a.*y;
t1 = [t t].*X;
w = sum(t1)'
b = y(1) - X(1,:)*w;

(X*w + b) / norm(w) 
%predict new points:
yNew = sign(w'*[3;-1] + b);
```

## Extensions of SVMs

### Kernel Function

在前面的讨论中，我们认为样本在特征空间是线性可分的，然而在现实任务中，原始样本空间不一定是线性可分的，最常见的例子就是“异或”。对于这样的问题，我们可以考虑从原始空间映射到更高维的特征空间，即将样本“升维”。

令\\(\phi(x)\\)为\\(x\\)升维后的表示，则相应形式的SVM原问题为：

$$ \min\_{w,b} \ \  \frac{1}{2}||w||_{2}^{2}  \\\\ s.t. \ \ y\_i(w^T\phi(x\_i) + b) \geq 1$$


对偶问题为：

$$\max\_{\alpha} L(w,b,\alpha) = -\frac{1}{2}\sum\_{i=1}^{m}\sum\_{j=1}^{n}\alpha\_i\alpha\_jy\_iy\_j{\phi(x\_i)}^T\phi(x\_j) + \sum\_{i=1}^{m} \alpha\_i$$

相应的对偶问题的约束条件为：

$$\alpha\_i \geq 0 \\\\ \sum\_{i=1}^{m}\alpha\_iy\_i = 0$$

可以发现在上面的等式中存在\\({\phi(x\_i)}^T\phi(x\_j)\\)的计算，然而由于升维后的空间维度可能很高，因此直接在升维后的特征空间计算内积通常是很困难的。在这里我们引入核函数(kernel function) 使得\\(k(x\_i,x\_j) = {\phi(x\_i)}^T\phi(x\_j)\\), 即将在升维后空间的内积转化为在原空间通过核函数计算的结果，相应的有：

$$\max\_{\alpha} L(w,b,\alpha) = -\frac{1}{2}\sum\_{i=1}^{m}\sum\_{j=1}^{n}\alpha\_i\alpha\_jy\_iy\_jk(x\_i,x\_j) + \sum\_{i=1}^{m} \alpha\_i$$

需要注意的是核函数要求其核矩阵是半正定的。常见的核函数有：

- 线性核：\\(k(x\_i,x\_j) = x\_i^Tx\_j\\)
- 多项式核: \\(k(x\_i,x\_j) = (x\_i^Tx\_j)^d\\)
- 高斯核: \\(k(x\_i,x\_j) = \exp(-\frac{||x\_i-x\_j||^2}{2\sigma^2})\\)



### Soft-margin SVM

在前面的讨论中，我们一直假定样本空间是线性可分的，然而现实任务中往往很难确定训练样本在特征空间中是否是线性可分。解决这个问题的办法是引入软间隔的概念(soft-margin)。在之前的问题中，我们认为所有点均需满足\\(y\_i(w^Tx\_i+b) \geq 1\\), 这个间隔太硬了，我们称之为硬间隔(hard-margin).

为了解决硬间隔的问题，我们允许可以有点不满足硬间隔的条件，即\\(y\_i(w^Tx\_i + b) \geq 1 - \xi\_i\\), 同时要求在最大化margin的同时，不满足硬间隔约束的点尽可能少，即\\(\min\_{w,b,\xi}\frac{1}{2}w^Tw + C\sum\_{i=1}^{m} \xi\_i\\),综上可得：

$$\min\_{w,b,\xi} \frac{1}{2}w^Tw + C\sum\_{i=1}^{m} \xi\_i \\\\ s.t. \ \ y\_i(w^Tx\_i + b) \geq 1 - \xi\_i \\\\ \xi\_i \geq 0$$

需要提到的一点是，目标函数中\\(\sum\_{i=1}^{m} \xi\_i \\) 的引入实际上采用的是hinge loss: \\(\max(0,1-z)\\).

对soft-margin SVM求解可以通过求其对偶问题的。soft-margin SVM相应的拉格朗日表达式为：

$$L(w,b,\alpha,\xi,\mu) = \frac{1}{2}w^Tw + C\sum\_{i=1}^{m} \xi\_i + \sum\_{i=1}^{m}\alpha\_i(1-\xi\_i-y\_i(w^Tx\_i+b)) - \sum\_{i=1}^{m}\mu\_i\xi\_i$$

相应的KKT条件为：

$$\alpha\_i \geq 0, \ \ \ \mu\_i \geq 0 \\\\ y\_i(w^Tx\_i + b) \geq 1 - \xi\_i \\\\ \xi\_i \geq 0 \\\\ \alpha\_i(1-\xi\_i-y\_i(w^Tx\_i+b)) = 0 \\\\ \mu\_i\xi\_i = 0$$

对\\(w,b,\xi\\)求偏导可得：

$$w = \sum\_{i=1}^{m} \alpha\_ix\_iy\_i $$

$$\sum\_{i=1}^{m}\alpha\_iy\_i = 0$$

$$C = \alpha\_i + \mu\_i$$


得出的对偶问题形式为：

$$\max\_{\alpha} \sum\_{i=1}^{m}\alpha\_i -\frac{1}{2} \sum\_{i=1}^{m}\sum\_{j=1}^{n}\alpha\_i\alpha\_jy\_iy\_jx\_i^Tx\_j \\\\ s.t. 0 \leq \alpha\_i \leq C \\\\ \sum\_{i=1}^{m} \alpha\_iy\_i = 0$$

若\\(\alpha\_i > 0\\) 则有 \\(y\_i(w^Tx\_i + b) = 1 - \xi\_i\\), 相应的样本均为support vectors. 此时若\\(\alpha\_i < C\\), 则\\(\mu\_i > 0\\), \\(\xi\_i = 0\\),可知该样本恰好在margin平面上；若\\(\alpha\_i ＝ C\\), 则\\(\mu\_i ＝ 0\\),此时若\\(\xi\_i < 1\\),可知该样本在最大间隔内部；若\\(\xi\_i > 1\\),可知该样本分类错误。










# Reference

- Book: [Pattern Recognition and Machine Learning by Christopher Bishop](prml)
- [Notes of machine learning by Andrew Ng in cousera](http://www.holehouse.org/mlclass)
- [机器学习][url1] 周志华
- Slides by [Prof. Wang](http://sist.shanghaitech.edu.cn/StaffDetail.asp?id=334)
- [统计学习方法](http://www.hangli-hl.com/books.html)

[url1]:http://cs.nju.edu.cn/zhouzh/zhouzh.files/publication/MLbook2016.htm
[prml]:http://www.rmki.kfki.hu/~banmi/elte/Bishop%20-%20Pattern%20Recognition%20and%20Machine%20Learning.pdf


<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>
