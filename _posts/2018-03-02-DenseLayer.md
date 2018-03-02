---
layout: post
title:  "Dense Layer / Fully Connected Layer（全結合層）"
---

https://arxiv.org/abs/1409.3215

## Model Structure
![Figure1](https://huitclub.github.io/images/dense.jpg "")
　前の層の出力を受け取り、重みとバイアスの計算処理を行い、次の層（活性化関数の層）へ伝播させる。


## Forward
**input:** $$ \boldsymbol{z}^{(l-1)} = \left( z^{(l-1)}_i \right) $$

**parameter:**

weight: $$\boldsymbol{W}^{(l)} = \left( w^{(l)}_{ji} \right)$$

bias: $$ \boldsymbol{b}^{(l)} = \left( b^{(l)}_j \right) $$

**formula:**

$$
\begin{align*}
\boldsymbol{u}^{(l)} = \boldsymbol{W}^{(l)} \boldsymbol{z}^{(l-1)} + \boldsymbol{u}^{(b)} 
\\
u^{(l)}_j = \sum_{i} w_{ji} z_{i} + b_{j}
\end{align*}
$$


**output:** $$ \boldsymbol{u}^{(l)} = \left( u^{(l)}_j \right) $$


## Backward
**input:** $$ \boldsymbol{\delta}^{(l)} = \left( \delta^{(l)}_j \right) $$

**formula:**

$$
\begin{align*}
\nabla_{\boldsymbol{W}^{(l)}} E = \boldsymbol{\delta}^{(l)} \left( \boldsymbol{z}^{(l-1)} \right)^{\top}
\\
\dfrac{\partial E}{\partial w^{(l)}_{ji}}= \dfrac{\partial E}{\partial u^{(l)}_{j}} \dfrac{\partial u^{(l)}_{j}}{\partial w^{(l)}_{ji}} = \delta^{(l)}_j z^{(l-1)}_i
\end{align*}
$$

$$
\begin{align*}
\nabla_{\boldsymbol{z}^{(l-1)}} E =  \left( \boldsymbol{W}^{(l)} \right)^{\top}
\boldsymbol{\delta}^{(l)}
\\
\dfrac{\partial E}{\partial z^{(l-1)}_{i}} = \sum_{k} \dfrac{\partial E}{\partial u^{(l)}_{k}}\dfrac{\partial u^{(l)}_{k}}{\partial z^{(l-1)}_{i}} = \sum_{k} \delta^{(l)}_j w^{(l-1)}_k
\end{align*}
$$


**output:** $$ \nabla_{\boldsymbol{W}^{(l)}} E =  \left( \dfrac{\partial E}{\partial w^{(l)}_{ji}}\right),  \nabla_{\boldsymbol{z}^{(l-1)}} E = \left( \dfrac{\partial E}{\partial z^{(l-1)}_{i}} \right)$$

## Tensorflow

```python
dense = tf.layers.dense(    inputs=z,              # z.shape = (column(i))    units=J,               # 隠れ層の unit 数(j)の指定    activation=tf.nn.relu  # activation function の指定)# -> shape = (column(j))
```