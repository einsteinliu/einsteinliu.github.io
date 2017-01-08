---
layout: post
title: SVM:multi-class SVM regularization 
description: "SVM"
tags: [SVM, Stanford Machine Learning Notes]
categories: [Machine Learning, Machine Learning Basic]
---

### Review 

For the ith sample $$(x_i,y_i)$$ in the training set, we have the following loss function:



$$L_i= \sum_{j≠y_i}max(0,w_j^T\cdot x_i−w_{y_i}^T\cdot x_i+Δ)$$

$$w_j^T\cdot x_i$$ is the score classifying $$x_i$$ to class j，and $$w_{y_i}^T\cdot x_i$$ is the score classifying correctly(classify to class $$y_i$$)，$$\omega_i$$ is the $$j$$th row of $$W$$.

### Problem

Problem 1:

Considering the geometrical meaning of the weight vector $$\omega$$, it is easy to find out that $$\omega$$ is not unique, $$\omega$$ can change in a small area and result in the same $$L_i$$. 

Problem 2:

It the values in $$\omega$$ is scaled, the loss computed will also be scaled by the same ratio. Considering a loss of 15, if we scale all the weights in $$\omega$$ by 2, the loss will be scaled to 30. But this kind of scaling is meaningless, it doesn't really represent the **loss**. 



<!-- more -->

### L2 Regularization

The method to solve the above two problems is adding a penalty term into the loss function to penalize the weight matrix:



$$
R(W)=||W||^2=\sum{W\cdot W}
$$


The penalized loss function turns to be:


$$
L=\frac{1}{N}\sum_iL_i+\lambda R(\omega)
$$

We can understand the first term as "**data loss**" term, its physical meaning is the summation of all loss values when applying the weight matrix $$W$$ to all data samples.

The second the term is the so called "**regularization loss**", it represent the loss from the weight matrix $$W$$'s own structure, which means this term is only related to the weight matrix and **not related to the sample data**.

The equation after the full expansion is:


$$
L=\frac{1}{N}\sum_i \sum_{j≠y_i}max(0,w_j^T\cdot x_i−w_{y_i}^T\cdot x_i+Δ)+\lambda\sum{W\cdot W}
$$

Let's review the meaning of the symbols:

$$N$$ means there are N samples in the training set.

$$i$$ means the $$i$$th sample in all these $$N$$ samples.

$$y_i$$ means the class of the $$i$$th sample

$$j$$ means all the other classes except class $$y_i$$ in all K classes

$$\omega_j$$ means the $$j$$th row of the weight matrix $$W$$, the same for $$\omega_{y_i}$$

$$\Delta$$ is the confidential interval.

$$\lambda$$ is the regularization ratio.

**Usually we use** $$\frac{1}{2}\lambda\omega^2$$ **as the regularization term, because** when updating the $$\omega$$, we need to compute the gradient respect to $$\omega$$, by adding a $$\frac{1}{2}$$ in front the gradient of the regularization term will be $$\lambda\omega$$ instead of $$2\lambda\omega$$.

This regularization on $$\omega^2$$ is the so called L2 regularization, adding this to the loss function means for every update step, **the weights** $$\omega$$ **is decayed linearly**:

$$W=+-lambda*W$$

### Why regularization can solve the problems?

For problem 1, as we added the loss of the weight matrix itself, we can finally get an unique solution.

For problem 2, using the weight matrix's $$L_2$$ form as the penalty term, **means we want the weights in the weight matrix to be small**, which prevents the weights in $$W$$ from meaningless scaling.

**The smaller is the weight matrix's $$L_2$$ form, we know that the smaller will be the weights in $$W$$, what's more, it also contains another information: the weights in $$W$$ will distributed more equally:**

The summation of vector $$\omega_1:\{1,0,0,0\}$$  and the vector $$\omega_2:\{0.25,0.25,0.25,0.25\}$$ are the same, but their $$L_2$$ norms are quite different, $$\omega_1's\ L_2$$  norm is 1 but $$\omega_2's\ L_2$$  norm is 0.25. If both of them can all do the classification job correctly, we apparently will choose $$\omega_2$$, which has the smaller penalty, because its distribution is more equal, and **the more equally distributed weight vector means all the pixels in a sample image are considered.** $$\omega_1$$'s classification result may also be good, but it only consider the 1st pixel, which gets a weight of 1, the left 3 pixels get the weights of 0, which means they are not considered for classification. 

But why we can still do the classification correctly without considering the other 3 pixels? **It means the training samples in our training set is very special: the 1st pixels in all the samples occasionally contains enough information for us to do the classification, but in the real case, all the 4 pixels are equally important. Now if we want to classify a new sample image whose 1st pixel doesn't contain enough information to classify the image, the $$\omega_1$$ can not classify this image anymore, this is the so-called "weak generalization ability" . If we choose $$\omega_2$$, it will still work, as it takes all the 4 pixels into consideration.**

### L1 Regularization

L1 regularization means adding $$\lambda\|\omega\|$$, it is also possible to combine L1 and L2 like:

$$
\lambda_1|\omega|+\lambda_2\omega^2
$$

which is called elastic net regularization. As we said that L2 regularization makes the $$\omega$$ decays linearly, for L1 regularization the weights will be reduced by a constant in every update step. Finally all the none-important weights will be disappeared and only the important weights will be left. Which means by using L1 regularization the system will be invariant to the "noisy" inputs. In comparison, using L2 will lead to diffuse, small weights.