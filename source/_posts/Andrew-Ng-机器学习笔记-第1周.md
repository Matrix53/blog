---
title: Andrew Ng 机器学习笔记(第1周)
date: 2022-04-18 21:54:32
categories: 人工智能
excerpt: 机器学习课程第1周的笔记，本周的内容包括课程介绍、单变量线性回归、线性代数复习三部分。
index_img: /img/AI/ai_index_1.jpg
banner_img: /img/AI/ai_banner_1.jpg
math: true
---

## 前言

这个系列的博客将作为我机器学习课程的笔记，部分用中文较难表述的词句我将使用英文原文。

第 1 周的内容比较简单，内容也非常少，分为课程介绍、单变量线性回归、线性代数复习三部分。

## 课程介绍

机器学习的定义：

- **非形式化定义**：不显式编程，使计算机具有学习能力的研究领域

- **形式化定义**：若计算机程序在任务 T 上的性能(由 P 衡量)用经验 E 来提高，则称该程序从经验 E 中学习任务 T 和性能度量 P

机器学习可以分为有监督学习和无监督学习两类：

- **有监督学习**：
  已知输入和输出，求从输入到输出的映射关系。有监督学习问题分为**回归**和**分类**问题。
  在回归问题中，输出是连续的值，如房价预测；在分类问题中，输出是离散的值，如图像分类。

- **无监督学习**：
  已知输入(无标签数据集)，求输入的内在结构。无监督学习问题分为**聚类**和**非聚类**问题。
  给定基因数据集，将其分为多类，是聚类问题；给定高维数据集，将其降至低维，是非聚类问题。

## 单变量线性回归

这一节简单讲解了有监督学习中的回归问题，以确定房价预测函数作为例子，先后完成了模型表示、确定代价函数、梯度下降寻找最优解三个步骤，笔记如下：

- **平方误差函数**：$$J(\theta_0,\theta_1)=\frac{1}{2m}\Sigma_{i=1}^{m}(h(x_i)-y_i)^{2}$$其中，$m$为样本容量，$x_i,y_i$为第$i$个样本的输入和输出，$h$为待定函数(自变量为$x$)，$\theta_0,\theta_1$为$h$的两个待定系数

- **梯度下降公式**：$$\Theta:=\Theta-\alpha\frac{\partial}{\partial\Theta}J(\Theta)$$其中，$:=$为赋值符号，$J$为代价函数，$\Theta$向量为$J$的参数，$\Theta$向量也是待定函数$h$的待定系数，$\alpha$为学习率

## 线性代数复习

这一节的内容是线性代数基础，给出了矩阵和向量的定义，并定义了矩阵的相关运算。由于内容过于基础，就不做笔记了。如有还未学习过线性代数的读者，我推荐观看[3Blue1Brown 的线性代数合集](https://www.bilibili.com/video/BV1ys411472E)。

## 相关链接

- Coursera：Andrew Ng[机器学习课程](https://www.coursera.org/learn/machine-learning)
- 知乎：[常用聚类算法](https://zhuanlan.zhihu.com/p/104355127)

由于作者水平有限，所以文章中难免有少数不严谨之处，如有读者发现此类疏忽，恳请读者指出。另外，如果认为本文对您有帮助，欢迎请作者喝咖啡！![Matrix53的微信赞赏码](/img/global/wxQRcode_pay.png)
