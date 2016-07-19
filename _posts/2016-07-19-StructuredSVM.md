---
layout: post
title: Structural SVM with Latent Variables 论文笔记
description: "Structural SVM论文笔记"
tags: [线性分类器, 论文笔记, 机器学习, Optimization, SVM]
categories: [Machine_Learning, Machine_Learning_Basic]
---

Structural SVM是SVM的一个变体，下文简写为SSVM

#### SSVM的特殊预测函数

先回顾一下普通的SVM的预测函数：

$$f(x)=sgn((ω\cdot x)+b) $$

ω是权重向量，x是输入值，b是偏置，sgn是符号函数，f(x)是预测结果

SSVM的一大特点就是其特别的预测函数：

$$f_ω (x)=argmax_{y∈Υ} [ω\cdot Φ(x,y)]$$

y是可能的预测结果，Υ是y的搜索空间，而Φ是一个x和y的函数

Φ is the joint feature vector describes the relationship between x and y

于是给定ω，不同的x给出不同的预测

<!-- more -->

#### SSVM的特殊损失函数及其优化问题

在优化过程中，对于普通的SVM，我们需要定义损失函数，一般的Hinge Loss损失函数形式为：

$$[1−y_i (ω\cdot x_i+b)]_+$$

而针对此损失函数的优化问题是：

$$argmin_{(ω,b)}⁡\left\{ { \frac{1}{2}|ω|^2+C\sum_{i=1}^N[ 1−y_i (ω\cdot x_i+b)]_+}\right\} $$


对于SSVM，损失函数更为复杂：

论文中的风险函数亦即损失函数，损失函数是计算预测值和实际值差别的函数，是一个自定义的函数，表示如下：

$$Δ(y,\hat{y})$$

对于给定的特征$$x_i$$
我们也可以认为对于$$x_i$$的预测值$$\hat{y_i}$$是ω的函数：

$$\hat{y_i}(ω)=argmax_{y∈Υ} [ω\cdot Φ(x_i,y)]$$


若针对自定义的损失函数，需要解决以下优化问题：

$$argmin_{(ω,b)}⁡\left\{ { \frac{1}{2}|ω|^2+C\sum_{i=1}^NΔ(y_i,\hat{y_i} (ω))}\right\} $$

不同于hinge loss函数的是，此处使用的损失函数

$$Δ(y_i,\hat{y_i}(ω))$$

对于变量ω是非凸的，故此问题不可解。

为何非凸？因为$$\hat{y_i}(ω)$$十分复杂，他来自于一个离散空间$$Υ$$的搜索，不是任何可以用公示表达的凸函数。

所以我们需要将此非凸优化问题转化为凸优化问题

#### 将SSVM的非凸优化问题转化为凸优化问题

考虑以下不等关系：

$$Δ(y_i,\hat{y_i}(ω)) \leqslant Δ(y_i,\hat{y_i}(ω)) + ω\cdot Φ(x_i,\hat{y_i}(ω))-ω\cdot Φ(x_i,y_i)$$ 


因为对于确定的$$x_i$$ 和ω：

$$ω\cdot Φ(x_i,\hat{y_i}(ω))$$是最大值，故

$$ω\cdot Φ(x_i,\hat{y_i}(ω))−ω\cdot Φ(x_i,y_i)$$必然大于等于0


进一步的：

$$
\begin{align*}
& Δ(y_i,\hat{y_i}(ω))+ω\cdot Φ(x_i,\hat{y_i}(ω))−ω\cdot Φ(x_i,y_i)\\
& \leqslant max_{y∈Υ}[Δ(y_i,y)+ω\cdot Φ(x_i,y) - ω\cdot Φ(x_i,y_i)]因左式中最后一项与y无关，故可移出max:\\
& \leqslant max_{y∈Υ}[Δ(y_i,y)+ω\cdot Φ(x_i,y)] - ω\cdot Φ(x_i,y_i)\\
\end{align*}
$$

于是：

$$Δ(y_i,\hat{y_i}(ω))\leqslant max_{y∈Υ}[Δ(y_i,y)+ω\cdot Φ(x_i,y)] - ω\cdot Φ(x_i,y_i)$$

以上即为论文中的转化过程

关键部分，损失函数变为：

$$max_{y∈Υ}[Δ(y_i,y)+ω\cdot Φ(x_i,y)] - ω\cdot Φ(x_i,y_i)$$

这是一个对于变量ω的凸函数，因为有定理：

**凸函数的上界也是凸函数**

对于y的取值空间$Υ$，每一个y都对应了一个凸函数$$ω\cdot Φ(x_i,y)$$

所有这些凸函数的上界也是凸函数，而对于ω而言，第一项$$Δ(y_i,y)$$是常量

所以对于ω而言，此损失函数为凸函数

又**因为凸函数的和也是凸函数**，于是转化之后的问题：

$$argmin_{(ω,b)}⁡\left\{ { \frac{1}{2}|ω|^2+C\sum_{i=1}^N (\,  max_{y∈Υ}[Δ(y_i,y)+ω\cdot Φ(x_i,y)] - ω\cdot Φ(x_i,y_i)} )\, \right\} $$

就成了一个凸优化问题

论文的后半部分，解决带Latent Variable的SSVM问题，使用的是相同的思路

论文链接：

[Learning Structural SVMs with Latent Variables](http://www.cs.cornell.edu/~cnyu/papers/siso_workshop.pdf)



