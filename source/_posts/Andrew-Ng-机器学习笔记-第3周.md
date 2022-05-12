---
title: Andrew Ng 机器学习笔记(第3周)
math: true
date: 2022-05-10 15:36:48
categories: 人工智能
excerpt: 机器学习课程第3周的笔记，本周的内容包括分类问题及其表示、逻辑回归模型、多类分类问题、解决过拟合四部分。
index_img: /img/AI/ai_index_3.jpg
---

## 前言

这个系列的博客将作为我机器学习课程的笔记，部分用中文较难表述的词句我将使用英文原文。

第 3 周的课程标题是分类问题，包括分类问题及其表示、逻辑回归模型、多类分类问题、解决过拟合四部分。

## 分类问题及其表示

**Sigmoid 函数**(Logistic 函数)：将$[-\infty, \infty]$映射到$[0, 1]$，可用于将数值转化为概率，其函数表达式如下。

$$
g(z)=\frac{1}{1+e^{-z}}
$$

**决策边界**([Decision Boundary](https://en.wikipedia.org/wiki/Decision_boundary))：在二分类问题中，决策边界是一个将向量空间一分为二的超平面，在决策边界两侧的点属于不同的类别。

## 逻辑回归模型

待优化的$J(\Theta)$是由$Cost(h_{\theta}(x),y)$和$h_{\theta}(x)$共同决定的，当$J(\Theta)$是[凸函数](https://en.wikipedia.org/wiki/Convex_function)时，可以找到**全局最优解**。

在**逻辑回归**(Logistic Regression)模型中，有如下公式：

$h_{\theta}(x)=g(\Theta^T x)$

$Cost(h_{\theta}(x),y)=-y\log(h_{\theta}(x))-\left(1-y\right)\log(1-h_{\theta}(x))$

$\theta_j=\theta_j-\frac{\alpha}{m}\Sigma_{i=1}^{m}(h_{\theta}(x^{(i)})-y^{(i)})x_j^{(i)}$

除了梯度下降算法**之外**，还有很多算法能够优化$\Theta$，例如 [Conjugate Gradient](https://en.wikipedia.org/wiki/Conjugate_gradient_method)、[BFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm)、[L-BFGS](https://en.wikipedia.org/wiki/Limited-memory_BFGS) 等，建议使用**库函数**而不是手写这些算法。

## 多类分类问题

**One-vs-all**：对于有$n$个类的分类问题，可以训练$n$个逻辑回归分类器，第$i$个分类器预测$y=i$的概率，最后选择这些概率中**最大值所在的类**作为预测结果。

## 解决过拟合

**欠拟合**([underfitting](https://en.wikipedia.org/wiki/Overfitting#Underfitting)、high bias)：算法在训练集上表现不好，没有学习到数据的内在结构。

**过拟合**([overfitting](https://en.wikipedia.org/wiki/Overfitting)、high variance)：算法在训练集上表现好，但在测试集上表现不好，泛化性能差。

可以通过**减少特征的数量**或者**正则化**等方法，来解决过拟合问题。

**正则化**([Regularization](https://en.wikipedia.org/wiki/Regularization_(mathematics)))：将最终拟合出的函数“简单化”的方法，分为显式正则化和隐式正则化。

## 参考资料

- Coursera：Andrew Ng[机器学习课程](https://www.coursera.org/learn/machine-learning)
- 知乎：[常用聚类算法](https://zhuanlan.zhihu.com/p/104355127)

由于作者水平有限，所以文章中难免有少数不严谨之处，如有读者发现此类疏忽，恳请读者指出。另外，如果认为本文对您有帮助，欢迎请作者喝咖啡！![Matrix53的微信赞赏码](/img/global/wxQRcode_pay.png)
