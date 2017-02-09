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


### 多变量线性回归(Multivariate Linear Regression)
目前为止，我们探讨了单变量/特征的回归模型，现在我们对房价模型增加更多的特征，例如
房间数楼层等，构成一个含有多个变量的模型，模型中的特征为 $x_{1},x_{2},...,x_{n}$ 。

![linear4](/images/linear4.png)

增添更多特征后，我们引入一系列新的注释：

$$\begin{align*}x_j^{(i)} &= \text{value of feature } j \text{ in the }i^{th}\text{ training example} \newline x^{(i)}& = \text{the column vector of all the feature inputs of the }i^{th}\text{ training example} \newline m &= \text{the number of training examples} \newline n &= \left| x^{(i)} \right| ; \text{(the number of features)} \end{align*}$$

支持多变量的假设 h(hypothesis) 表示为：

#### $$\begin{align*}h_\theta (x) = \theta_0 + \theta_1 x_1 + \theta_2 x_2 + \theta_3 x_3 + \cdots + \theta_n x_n\end{align*}$$

该公式可转化为特征矩阵：

#### $$\begin{align*}h_\theta(x) =\begin{bmatrix}\theta_0 \hspace{2em} \theta_1 \hspace{2em} ... \hspace{2em} \theta_n\end{bmatrix}\begin{bmatrix}x_0 \newline x_1 \newline \vdots \newline x_n\end{bmatrix}= \theta^T x\end{align*}$$

#### 多变量梯度下降(Gradient Descent for Multiple Variables)
与单变量线性回归类似，在多变量线性回归中，我们只需重复我们的 'n' 个特征：

##### $$\begin{align*} & \text{repeat until convergence:} \; \lbrace \newline \; & \theta_0 := \theta_0 - \alpha \frac{1}{m} \sum\limits_{i=1}^{m} (h_\theta(x^{(i)}) - y^{(i)}) \cdot x_0^{(i)}\newline \; & \theta_1 := \theta_1 - \alpha \frac{1}{m} \sum\limits_{i=1}^{m} (h_\theta(x^{(i)}) - y^{(i)}) \cdot x_1^{(i)} \newline \; & \theta_2 := \theta_2 - \alpha \frac{1}{m} \sum\limits_{i=1}^{m} (h_\theta(x^{(i)}) - y^{(i)}) \cdot x_2^{(i)} \newline & \cdots \newline \rbrace \end{align*}$$

同理，多变量线性回归的批量梯度下降算法为：

##### $$\begin{align*}& \text{repeat until convergence:} \; \lbrace \newline \; & \theta_j := \theta_j - \alpha \frac{1}{m} \sum\limits_{i=1}^{m} (h_\theta(x^{(i)}) - y^{(i)}) \cdot x_j^{(i)} \; & \text{for j := 0...n}\newline \rbrace\end{align*}$$

#### 特征缩放(Feature Scaling)
在我们面对多维特征问题的时候，我们要保证这些特征都具有相近的尺度，这将帮助梯度下降算法更快地收敛。
以房价问题为例，假设我们使用两个特征，房屋的尺寸和房间的数量，尺寸的直为0-2000平方英尺，而房间数量的直则是 0-5，以两个参数分别为横纵坐标，绘制代价函数的等高线图能看出图像会显得很扁，梯度下降算法需要非常多次的迭代才能收敛。

![linear5](/images/linear5.png)

解决的方法是尝试将所有特征的尺度都尽量缩放到-1 到 1 之间。最简单的方法是令：

#### $$x_i := \dfrac{x_i - \mu_i}{s_i}$$

其中$\mu_i$是平均值，${s_i}$是标准差。

#### 学习率$\alpha$(Learning Rate)
调试梯度下降：绘制迭代次数和代价函数的图表来观测算法在何时趋于收敛，横轴为迭代次数，纵轴为$J(\theta)$，如果$J(\theta)$有曾增加，我们需要减小$\alpha$。
自动收敛测试：如果代价函数的变化值比预先设定的阀值（例如0.001）小，就认为收敛。但实际操作中很难选定阈值。

![linear6](/images/linear6.png)

总结来说，梯度下降算法的每次迭代受到学习率的影响，如果学习率$\alpha$过小，则达到收敛所需的迭代次数会非常高；如果学习率$\alpha$过大，每次迭代可能不会减小代价函数，可能会越过局部最小值导致无法收敛。