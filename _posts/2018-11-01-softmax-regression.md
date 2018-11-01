---
layout: post
title: 理解 Softmax Regression 与 Logistic Regression
mathjax: true
comments: true
abstract: >-
  Softmax 函数广泛用于各种多类分类模型中，它是 Logistic 函数的一个推广形式。尽管形式上两者看似差距甚大，但本质却是一致的。如果把 Logistic 函数看成是 Softmax 函数在二类任务时的情况，而不是把后者看成是前者的推广形式，就能更清晰地捕捉两者之间的关联。
---

> [Softmax Regression - Stanford](http://ufldl.stanford.edu/tutorial/supervised/SoftmaxRegression/) 中详细介绍了 Softmax Regression 以及它与 Logistic Regression 之间的关系。  
> 这里只是简单的补充（Softmax 函数的直观意义）与整理。

### 1. Softmax Regression 与 Logistic Regression

将一件事情发生的概率记为 $p$，不发生的概率则为 $1 - p$，两者的比值即为几率 (odds)：

$$\text{odds} = \frac{p}{1 - p}$$

几率衡量了该事情发生的相对可能性。在 Softmax Regression 模型中，我们将**对数几率 (log-odds)** 看作是**特征 $x$ 的线性函数所表示的模型**：

$$\text{log-odds} = \log \frac{p}{1 - p} = \theta \cdot x$$

此时几率则可表达为：

$$\text{odds} = \exp(\theta \cdot x)$$

这时我们再来看 Softmax Regression 在 K 类分类任务时的 hypothesis 函数：

$$\begin{align}
h_\theta(x) =
\begin{bmatrix}
P(y = 1 | x; \theta) \\
P(y = 2 | x; \theta) \\
\vdots \\
P(y = K | x; \theta)
\end{bmatrix}
=
\frac{1}{ \sum_{j=1}^{K}{\exp(\theta^{(j)} \cdot x) }}
\begin{bmatrix}
\exp(\theta^{(1)} \cdot x ) \\
\exp(\theta^{(2)} \cdot x ) \\
\vdots \\
\exp(\theta^{(K)} \cdot x ) \\
\end{bmatrix}
\end{align}$$

也就是说，$h_\theta(x)$ 的每个值是各自类别的几率大小，然后使用 $\frac{1}{\sum_{j=1}^K \exp(\theta^{(j)} \cdot x)}$ 进行归一化处理，以使所有类别的输出几率之和为 1。归一化之后的几率可以看作是各个类别的概率。这便是 Softmax Regression 的直观意义所在。

[Softmax Regression - Stanford](http://ufldl.stanford.edu/tutorial/supervised/SoftmaxRegression/) 的第三节提到了 Softmax Regression 的一个属性，即它有一个冗余的参数。具体来说，将所有的参数 $\theta^{(j)}$ 均减去一个同维向量 $\psi$ 后，并不会影响模型的输出 $h_\theta(x)$。因此可令 $\psi$ 等于任意一个 $\theta^{(j)}$，不失一般性，我们令 $\psi = \theta^{(K)}$，这时模型的参数便只剩下 $\theta^{(1)}, \theta^{(2)}, \dots, \theta^{(k - 1)}$，而 $\theta^{(K)} = \mathbf{0}$，此时 Softmax Regression 的 hypothesis 函数可写作为：

$$\begin{align}
h_\theta(x) =
\begin{bmatrix}
P(y = 1 | x; \theta) \\
P(y = 2 | x; \theta) \\
\vdots \\
P(y = K | x; \theta)
\end{bmatrix}
=
\frac{1}{1 + \sum_{j=1}^{K-1}{\exp(\theta^{(j)} \cdot x) }}
\begin{bmatrix}
\exp(\theta^{(1)} \cdot x ) \\
\exp(\theta^{(2)} \cdot x ) \\
\vdots \\
1 \\
\end{bmatrix}
\end{align}$$

若令 $K = 2$，便可得到 **Logistic Regression**。

### 2. Cost Function

对 Softmax 函数采用极大似然估计法来估计各个参数，即可得到其 Cost Function：

$$J(\theta) = 
- \left[ \sum_{i=1}^{m} \sum_{k=1}^{K}  1\left\{y^{(i)} = k\right\} \log \frac{\exp(\theta^{(k)} \cdot x^{(i)})}{\sum_{j=1}^K \exp(\theta^{(j)} \cdot x^{(i)})}\right]$$

依然地，当 $K = 2$ 时（但二分类模型中类别 $K$ 的取值为 $\{0, 1\}$，$K$ 是从 0 开始，所以下式中写的是当 $K = 1$ 时），可以得到 Logistic Regression 的 Cost Function：

$$\begin{align}
J(\theta) &= - \left[ \sum_{i=1}^{m} \sum_{k=0}^{1} 1\left\{y^{(i)} = k\right\} \log P(y^{(i)} = k | x^{(i)} ; \theta) \right] \\
&= - \left[ \sum_{i=1}^m   (1-y^{(i)}) \log (1-h_\theta(x^{(i)})) + y^{(i)} \log h_\theta(x^{(i)}) \right]
\end{align}$$