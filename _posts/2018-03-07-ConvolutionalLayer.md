---
layout: post
title:  "Convolutional Layer （畳み込み層）"
date: 2018-03-07
category: [explanation]
tags: [DNN, CNN]
---

## Model Structure
<img src="https://huitclub.github.io/images/convolution.jpg" alt="Figure1" title="dense" height="300">

　全結合層と違い、結合が疎である結合層。一般に入力データは3次元で与えられる。計算過程は、[ここ](http://cs231n.github.io/convolutional-networks/)のImplementation as Matrix Multiplication.にてとてもわかりやすく視覚化されている。

## Forward
**input:** $$ \boldsymbol{z}^{(l-1)} = \left( z^{(l-1)}_{abk} \right), S : \rm{stride}$$

**parameter:**

weight: $$\boldsymbol{W}^{(l)} = \left( w^{(l)}_{pqkm} \right)$$

bias: $$ \boldsymbol{b}^{(l)} = \left( b^{(l)}_{ijm} \right) $$

**formula:**

$$
\begin{gather}
\boldsymbol{u}^{(l)} = \boldsymbol{z}^{(l-1)} \circledast \boldsymbol{W}^{(l)}  + \boldsymbol{b}^{(l)}
\\
u_{ijm} =\sum_{k} \sum_{p} \sum_{q}z_{Si+p,Sj+q,k}^{(l-1)} w_{pqkm}^{(l)} + b_{ijm}^{(l)}
\end{gather}
$$


**output:** $$ \boldsymbol{u}^{(l)} = \left( u^{(l)}_{ijm} \right) $$


## Backward
**input:** $$ \boldsymbol{\delta}^{(l)} = \left( \delta^{(l)}_{ijm} \right)= \left( \dfrac{\partial E}{\partial u^{(l)}_{ijm}} \right) $$

**formula:**

$$
\begin{gather}
\dfrac {\partial E}{\partial w_{pqkm}}=
\sum _{i}\sum_{j}\dfrac {\partial E}{\partial u_{ijm}^{(l)}}\dfrac {\partial u_{ijm}^{(l)}}{\partial w_{pqkm}^{(l)}} = \sum _{i} \sum_{j} \delta_{ijm}^{(l)}z_{Si+p,Sj+q,k}^{(l-1)}
\\
a = Si + p, b =Sj + q
\\
 \dfrac {\partial E}{\partial u_{abk}^{(l)}} = \sum_{m} \sum_{i} \sum_{j}\dfrac {\partial E}{\partial u_{ijm}^{(l)}}\dfrac {\partial u_{ijm}^{(l)}}{\partial z_{abk}^{(l-1)}} = \sum_{m} \sum_{i} \sum_{j} \sum_{p} \sum_{q} \delta^{(l)}_{ijm} w^{(l)}_{pqkm}
\end{gather}
$$


**output:** $$ \nabla_{\boldsymbol{W}^{(l)}} E =  \left( \dfrac{\partial E}{\partial w^{(l)}_{pqkm}}\right),  \nabla_{\boldsymbol{z}^{(l-1)}} E = \left( \dfrac{\partial E}{\partial z^{(l-1)}_{abk}} \right)$$

## Tensorflow

```python
import tensorflow as tf

conv = tf.layers.conv2d(    inputs=u,                 # u.shape = (width(a), height(b), channel(k))    filters=m,                # filter の number(m) を指定    kernel_size=[p, q],       # filter の size(p*q) を指定    strides=[Sn, Si, Sj, Sk], # batch_size, width, height, channel 方向に stride させる    padding="same",           # padding 方式の指定 (width * height を保つように自動調整)    activation=tf.nn.relu     # activation function の指定    ) # -> shape = (width_out(i), height_out(j), number(m))
```
