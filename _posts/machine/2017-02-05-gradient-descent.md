---
layout: page
title: 线性回归和梯度下降
subheadline: "Linear Regression & Gradient Descent"
teaser: "本编主要介绍机器学习中的线性回归问题，涉及到损失函数和梯度下降算法。"
categories:
    - machine
tags:
    - machine learning
---

### 线性回归(Linear Regression)

首先区分一下什么是监督式学习（Supervised Learning）和无监督性学习。所谓的监督式学习方法就是通过一组已有的样本训练出一个数学模型，然后将该模型用于预测或者分类，例如线性回归。而无监督式学习对没有概念标记（分类）的训练样本进行学习，以发现训练样本集中的结构性知识，例如聚类。

在统计学中，线性回归是利用称为线性回归方程的最小平方函数对一个或多个自变量和因变量之间关系进行建模的一种回归分析。这种函数是一个或多个称为回归系数的模型参数的线性组合。

回归分析中，只包括一个自变量和一个因变量，且二者的关系可用一条直线近似表示，这种回归分析称为一元线性回归分析。如果回归分析中包括两个或两个以上的自变量，且因变量和自变量之间是线性关系，则称为多元线性回归分析。

#### 模型表达(Model Representation)
以房屋交易问题为例，假使我们回归问题的训练集（Training Set）如下表所示：

![linear1](/images/linear1.png)

在该数据集中，只有一个自变量面积，和一个因变量价格，利用该数据集，我们的目的是训练一个线性方程，无限逼近所有数据点，然后利用该方程与给定的某一自变量（本例中为面积），可以预测因变量（本例中为房价）。我们将要用来描述这个回归问题的标记如下: 

+ $m$ 代表训练集中实例的数量 
+ $x$ 代表特征/输入变量 
+ $y$ 代表目标变量/输出变量 
- $(x,y)$ 代表训练集中的实例 
- $(x^{(i)},y^{(i)})$ 代表第 $i$ 个观察实例 
* $h$ 代表学习算法的解决方案或函数也称为假设 

同时，分析得到的线性方程为： 

#### \begin{equation}h_{\theta}=\theta_{0}+\theta_{1}x\end{equation}

### 损失函数(Cost Function)

为了得到目标线性方程，我们只需确定公式中的$\theta_{0}$和$\theta_{1}$，为了确定所选定的$\theta_{0}$和$\theta_{1}$效果好坏，通常情况下，我们使用一个损失函数(Cost Function)或者说是错误函数(Error Function)来评估$h_{\theta}$函数的好坏。损失函数如下： 

##### \begin{equation}J(\theta_{0},\theta_{1})=\frac{1}{2m}\sum_{i=1}^{m}(h_{\theta}(x^{(i)})-y^{(i)})^{2}\end{equation}

### 梯度下降(Gradient Descent)
梯度下降背后的思想是：开始时我们随机选择一个参数的组合 $(\theta_{0},\theta_{1},...,\theta_{n})$，计算代价函数，然后我们寻找下一个能让代价函数值下降最多的参数组合。我们持续这么做直到到到一个局部最小值，因为我们并没有尝试完所有的参数组合，所以不能确定我们得到的局部最小值是否是全局最小值，不同的初始参数组合，可能会得到不同的局部最小值。

![linear3](/images/linear3.png)

__批量梯度下降__(batch gradient descent)算法的公式为：

##### \begin{equation}\theta_{j}:=\theta_{j}-\alpha\frac{\partial}{\partial\theta_{j}}J(\theta_{0},\theta_{1}) \qquad(for \; j=0 \; and \ j=1)\end{equation}

其中$\alpha$是学习率（Learning Rate）,它决定了我们沿着能让代价函数下降程度最大的方向向下迈出的步子有多大，在批量梯度下降中，我们每一次都同时让所有的参数减去学习速率乘以代价函数的导数。

#### __对线性回归运用梯度下降法__
对我们之前的线性回归问题运用梯度下降法，关键在于求出代价函数的导数，即：

##### \begin{equation}\frac{\partial}{\partial\theta_{j}}J(\theta_{0},\theta_{1})=\frac{1}{2m}\frac{\partial}{\partial\theta_{j}}\sum_{i=1}^{m}(h_{\theta}(x^{(i)})-y^{(i)})^{2}\end{equation}

带入$j=0$，$j=1$ 得：

##### \begin{equation}\frac{\partial}{\partial\theta_{0}}J(\theta_{0},\theta_{1})=\frac{1}{m}\sum_{i=1}^{m}(h_{\theta}(x^{(i)})-y^{(i)}) \qquad(j=0)\end{equation}

##### \begin{equation}\frac{\partial}{\partial\theta_{1}}J(\theta_{0},\theta_{1})=\frac{1}{m}\sum_{i=1}^{m}\big((h_{\theta}(x^{(i)})-y^{(i)})(x^{(i)})\big) \quad(j=1)\end{equation}

进一步得到

##### \begin{equation}\theta_{0}:=\theta_{0}-\alpha\frac{1}{m}\sum_{i=1}^{m}(h_{\theta}(x^{(i)})-y^{(i)})\end{equation}

##### \begin{equation}\theta_{1}:=\theta_{1}-\alpha\frac{1}{m}\sum_{i=1}^{m}\big((h_{\theta}(x^{(i)})-y^{(i)})(x^{(i)})\big)\end{equation}

其中要求$\theta_{0}$ 和 $\theta_{1}$ 同步更新。