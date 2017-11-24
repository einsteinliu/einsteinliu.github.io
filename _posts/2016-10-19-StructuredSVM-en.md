---
layout: post
title: Structural SVM with Latent Variables Notes
description: "Structural SVM notes"
tags: [Paper Notes, Machine Learning, Optimization, SVM]
categories: [Machine Learning, Machine Learning Basic]
---

Structural SVM is a variation of SVM, hereafter to be refered as SSVM 

### Special prediction function of SSVM

Firstly let's recall the normal SVM's prediction function:

$$f(x)=sgn((ω\cdot x)+b) $$

ω is the weight vector，x is the input，b is the bias，$$sgn$$ is sign function，$$f(x)$$ is the prediction result.

On of SSVM's specialties is its prediction function：

$$f_ω (x)=argmax_{y∈Υ} [ω\cdot Φ(x,y)]$$

y is the possible prediction result，Υ is y's searching space，and Φ is some function of x and y.Φ will be a joint feature vector describes the relationship between x and y

Then for some given $$\omega$$, different prediction will be made according to different x.

<!-- more -->

### SSVM's special loss function and its optimization problem

For a normal SVM, we use the Hinge Loss as the loss function:



$$[1−y_i (ω\cdot x_i+b)]_+$$

Its optimization problem will be:

$$argmin_{(ω,b)}⁡\left\{ { \frac{1}{2}|ω|^2+C\sum_{i=1}^N[ 1−y_i (ω\cdot x_i+b)]_+}\right\} $$

for SSVM, the loss function will be more complicated,

The risk function in the paper is the loss function, loss function is the function to compute the difference between the prediction and the real value, it can be represented as:



$$Δ(y,\hat{y})$$

for a given $$x_i$$, we can also say that the prediction $$\hat{y_i}$$ of $$x_i$$ is a function of ω：



$$\hat{y_i}(ω)=argmax_{y∈Υ} [ω\cdot Φ(x_i,y)]$$

for the new loss function, our optimization problem turns to be:

$$argmin_{(ω,b)}⁡\left\{ { \frac{1}{2}|ω|^2+C\sum_{i=1}^NΔ(y_i,\hat{y_i} (ω))}\right\} $$

different from the hinge loss function, the loss function we use now:



$$Δ(y_i,\hat{y_i}(ω))$$

is not convex according to variable $$\omega$$, so this problem is not solvable.

Why is it non-convex? Because $$\hat{y_i}(ω)$$ is very complicated, it results from searching a discrete space $$Υ$$, which can not be expressed by any kind of convex function.

So we should firstly turn this non-convex problem to be convex.

### Turning SSVM's non-convex optimization problem to be convex

###### Considering the following non-equality:



$$Δ(y_i,\hat{y_i}(ω)) \leqslant Δ(y_i,\hat{y_i}(ω)) + ω\cdot Φ(x_i,\hat{y_i}(ω))-ω\cdot Φ(x_i,y_i)$$ 


for given $$x_i$$ and ω：

$$ω\cdot Φ(x_i,\hat{y_i}(ω))$$ is the max，so

$$ω\cdot Φ(x_i,\hat{y_i}(ω))−ω\cdot Φ(x_i,y_i)$$ must be bigger or equal to 0


step further:

$$
\begin{align*}
& Δ(y_i,\hat{y_i}(ω))+ω\cdot Φ(x_i,\hat{y_i}(ω))−ω\cdot Φ(x_i,y_i)\\
& \leqslant max_{y∈Υ}[Δ(y_i,y)+ω\cdot Φ(x_i,y) - ω\cdot Φ(x_i,y_i)] \\
& \leqslant max_{y∈Υ}[Δ(y_i,y)+ω\cdot Φ(x_i,y)] - ω\cdot Φ(x_i,y_i)\\
\end{align*}
$$

as the last term in the second line is not related to y, so it can be moved out of $$max$$.

finally we get：

$$Δ(y_i,\hat{y_i}(ω))\leqslant max_{y∈Υ}[Δ(y_i,y)+ω\cdot Φ(x_i,y)] - ω\cdot Φ(x_i,y_i)$$

The above described derivation shows the conversion.

The key part, our loss function turns to be:

$$max_{y∈Υ}[Δ(y_i,y)+ω\cdot Φ(x_i,y)] - ω\cdot Φ(x_i,y_i)$$

this is a convex function to $$\omega$$, cause we have a theorem:

**The upper bound of a convex function is also convex.**

For $$y$$'s value space $$Y$$, every $$y$$ gets its corresponding convex function:



 $$ω\cdot Φ(x_i,y)$$

The upper bounds of all these convex functions form a convex function, according to $$\omega$$, the first term $$Δ(y_i,y)$$ is a constant, so this loss function is convex to $$\omega$$.

What's more, we also know that:

**The summation of convex function is also convex**

Then the converted optimization problem:

$$argmin_{(ω,b)}⁡\left\{ { \frac{1}{2}|ω|^2+C\sum_{i=1}^N (\,  max_{y∈Υ}[Δ(y_i,y)+ω\cdot Φ(x_i,y)] - ω\cdot Φ(x_i,y_i)} )\, \right\} $$

turns to be a convex problem.

The second half of the paper solves the optimization problem with latent variables using the same technique.

Link of the paper:

[Learning Structural SVMs with Latent Variables](http://www.cs.cornell.edu/~cnyu/papers/siso_workshop.pdf)



