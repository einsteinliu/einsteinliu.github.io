---
layout: post
title: Linear Classification Note
description: "线性分类器笔记"
tags: [线性分类器, 斯坦福大学课程笔记]
categories: [Machine Learning, Machine Learning Basic]
---

### 打分函数

现在我们面对的是一个图片的分类问题，比如把一幅图分类为牛，猫。机器学习算法就是为一幅未分类的图片针对某一个类别打一个分数，然后根据分值大小来判断这幅图片是否属于这个类别。而分类算法的目标之一就是找到这个函数。

**Map/compute image pixels to the confidence score of each class**

Assume a training set:

$$(x_i,y_i)$$

$$x_i$$  is the image and $$y_i$$  is the corresponding class

i∈1…N means the traning set constains N images

$$y_i$$∈1…K means there are K image categories

**So a score function maps x to y:**

$$f(x_i,W,b)=W\cdot x_i+b$$

In the above function, each image $$x_i$$  is flattend to a 1 dimention vector

If one image's size is 32x32 pixels with 3 channels

$$x_i$$  will be a 1 dimention vector with the length of **D=32x32x3=3072**
Parameter matrix **W** has the size of **[KxD]**, it is often called weights
**b** of  size **[Kx1]** is often called bias vector
In this way, **W** is evaluating $$x_i$$'s confidence score for **K** categories at the same time


<!-- more -->

#### 得分函数的现实意义

**W**使用权值来判断每一个像素的重要性，比如一个用来给ship分类的**W**，可能在图片的四周赋给RGB通道中蓝色通道的权值就会比较大，因为一般ship的图片很可能是被蓝色的海洋包围的

#### 得分函数的几何意义

如果把一幅图$$x_i$$ 看做一个高维空间的一个点，那么显然**W**和b就组成了一个高维空间的超平面，**b**是截距，**W**是斜率，而把$$x_i$$ 代入公式后得到的值如果为正，说明$$x_i$$ 在超平面的“上方”，否则在“下方”

#### 得分函数的可视化意义

学习出来的**W**矩阵包含了其所对应的那一类图像的信息，准确的说，某一类图像的**W**也是一副图像，而这个图像的像素就是**W**所包含的权值，这个权值的含义就是这一类图像的这一个像素最有可能的值是什么。所以显然的，**W**的图像看起来很大略的像其所代表的那一类图像的样子：

![SVM_Visual]({{ site.url }}\images\machine_learning\linear\SVM_Visual.png)

上图中“马”这一类图像的权值图显示出了两个头的马，这意味着这个学习结果融合了向左和向右两种马的图像

上图中“汽车”这一类图像的权值图是红色的，这意味着我们的训练集中有太多红色的汽车了

这些因素都导致了学习的不精确和比较弱的泛化能力
​	
一般我们会把截距b也放到W里面，同时给$$x_i$$ 增加一维，填入1，于是

$$f(x_i,W,b)=W\cdot x_i+b$$

就变成了：

$$f(x_i,W)=W\cdot x_i$$

如下图所示：

![function]({{ site.url }}\images\machine_learning\linear\function.png)

#### 对原始数据的预处理

对于输入的图像，我们不能使用原始数据，一般要进行normalization：

首先计算出所有训练图像的平均图像，然后让每一幅训练图像都减去这幅平均图像，再把减完之后的结果（此时其像素应该大概在-127到127）继续压缩到-1到1之间


#### Hinge Loss 损失函数的确定

损失函数是指分在正确类别的分数和分在错误类别的分数之差：

比如，对于训练集中的第i个样本$$(x_i,y_i)$$：

$$L_i= \sum_{j≠y_i}max(0,w_j^T\cdot x_i−w_{y_i}^T\cdot x_i+Δ)$$

其中，$$w_j^T\cdot x_i$$是将$$x_i$$归于j类的得分，而$$w_{y_i}^T\cdot x_i$$ 是正确归类（归于$$y_i$$类）的得分，$$\omega_i$$ 是$$W$$的第$$j$$行。
​	
正常情况下，正确归类的得分是正的，而且很大，分为j类（错误分类）的得分是负的，而且很小
这样的话，错误分类的得分减去正确分类的得分，会得到一个很小的负数
也就是说，正常情况下，$$L_i$$ 应该是0，也就是没有损失。
​	
不正常的话，正确归类的的得分偏小，甚至是负的，而分为j类的得分可能是正的，甚至很大。
这样的话，错误分类的得分减去正确分类的得分，会得到一个很大的正数
那么在不正常的情况下，$$L_i$$会得到一个很大的正数，也就是说对于这个训练样本，损失很大
​	
那么Δ是什么呢？见下图：

 ![margin]({{ site.url }}\images\machine_learning\linear\margin.jpg)

当Δ=0我们只通过得分的绝对值大小来确定损失，这个条件是很松的，因为如果归到正确类别的得分比错误分类只大一点点，损失也几乎为零，事实上这种情况很容易混淆。

所以Δ其实是一个置信区间，也就是说，正确分类得分不仅要比错误分类得分高，还得高出Δ这么多才算完。如果正确分类和错误分类的得分相差很少，损失至少也是Δ。