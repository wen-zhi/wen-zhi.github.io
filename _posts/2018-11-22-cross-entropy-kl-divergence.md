---
layout: post
title: 轻松理解 Cross Entropy 与 KL 散度
mathjax: true
comments: true
excerpt: >-
  Cross Entropy 是机器学习算法中最为常用的损失函数之一。本文可以帮助你轻松地理解 Cross Entropy 的意义所在。
---

当一件事情发生的概率是 1/2 时，我们只需要向接收方传递 1 个 bit 就能告知结果：`1` or `0` （发生或不发生）。当然你也可以发送 `"Yes, it happened."` 这么长的字符，但它本质上值仍然只传递了 1 bit 的有效信息。

当一件事情发生的概率是 1/4 时，我们则只需要向接收方传递 2 个 bit。因为 2 个 bit 一共可以表达 4 种情况，传递 2 个 bit 可以表示这个事件是 4 个事件中的一个，它所占的概率为 1/4。

类似地，当一件事情发生的概率是 p 时，我们则需要传递 $$\log(1/p)$$ 个 bit。

**熵**就是求编码长度的期望：

$$H(p) = E(l_i) = E(\log(\frac{1}{p_i})) = \sum_i p_i \log(\frac{1}{p_i}) = -\sum_i p_i \log(p_i)$$

当你对一个事件使用 n 位 bit 的编码时，就是假定其发生的概率为 $\frac{1}{2^n}$。

假设现在对不同事件分别使用了不同长度的编码，于是可计算其各自对应的概率 $q_i$。也就是假定各事件的概率为此。但真实的各事件的概率为 $p_i$。如果按照 $q_i$ 所对应的编码长度来进行编码，其基于真实概率下的此种编码长度的期望为：

$$H(p, q) = \sum_i p_i \log(\frac{1}{q_i}) = -\sum_i p_i \log(q_i)$$

此即**交叉熵**（cross entropy）。

以多类分类为例：

	真实概率 p：    p1         p2        p3       p4        p5
	                ↕          ↕         ↕        ↕         ↕
	编码长度 l: log(1/q1) log(1/q2) log(1/q3) log(1/q4) log(1/q5) 
	                ↑          ↑         ↑        ↑         ↑
	预估概率 q:     q1         q2        q3       q4        q5

其交叉熵也就是各**真实概率**乘以**预估概率下得到的编码长度**以得到最终编码长度的期望。

编码长度越小，就越能更有效地传递信息。最小化交叉熵的过程也就是得到最优编码长度的过程。因此我们常见到机器学习模型使用交叉熵作为损失函数。

编码长度只是一种理解角度，在 Deep Learning (2016, Goodfellow et al.) 一书中则是将 $\log(\frac{1}{q_i})$ 称为 self-information。其 intuition 是，1) 如果一个事情发生的概率越大，那么我们能得到的信息就越少。比如“太阳每天东升西落”，其概率是 1，但我们得到的有用信息很少。2) 而那些概率小的事情能提供更多的信息。3)并且希望独立事件发生多次时，其得到的信息具有可加性，比如一件事件发生两次时，其得到的信息是可以累加的。将 self-information 定义为 $I(x) = log(\frac{1}{q})$，恰能满足这三条要求。

**熵**即最优情况下的编码长度，预估概率与真实概率差距较大时，其编码长度的均值（即交叉熵）会较大（易观察，可随便列举个例子，比如真实情况为 8 个事件，每个事件的概率都是 1/8，最佳编码长度为 3，如果使用其它预估概率来得到编码长度会出现什么结果？）。

交叉熵和熵的差值即为 KL 散度：

$$D_{KL}(p \| q) = H(p, q) - H(p)$$

KL 散度即表示了某种预估概率下的编码长度期望与最优编码长度下的期望差值，所以 KL 自然是越小越好。如果想要最小化 KL，则等价于最小化交叉熵。因为 $H(p)$ 是真实概率下的编码长度的期望，是个已知的固定值。

<br>

**References**

1. Aurélien Géron. [*A Short Introduction to Entropy, Cross-Entropy and KL-Divergence*](https://www.youtube.com/watch?v=ErfnhcEV1O8)
2. Ian Goodfellow et al. [*Deep Learning*](https://www.deeplearningbook.org/contents/prob.html)