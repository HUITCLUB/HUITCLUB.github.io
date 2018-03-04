---
layout: post
title:  "Softmax With Cross Entropy Layer"
date: 2018-03-03
---

## Model Structure
<img src="https://huitclub.github.io/images/softmax.jpg" alt="Figure1" title="softmax" height="300">

　Softmax関数は多クラス分類問題に使用される。最終層の出力を受け取り、各クラス$$C_k$$について入力$$\boldsymbol{x}$$が与えられた下での条件付き確率$$y_k = P\left(C_k \mid  \boldsymbol{x} \right)$$を計算する。<br>
　誤差関数には交差エントロピー誤差(cross entropy error)を用いる。交差エントロピーではone-hot表現で与えられる教師データ$$\boldsymbol{t} = (t_k)$$を用いて、誤差を計算する。

## Why use softmax?
　多クラス分類では、マルチヌーイ分布(multinoulli distribution)から得られる確率：

$$
\begin{equation}
P\left(y \mid  \boldsymbol{x} \right) =
\prod_{k=1}^{K} \left( P \left(C_k \mid  \boldsymbol{x} \right) \right)^{t_k}
= \prod_{k=1}^{K-1} \left( P \left(C_k \mid  \boldsymbol{x} \right) \right)^{t_k} \left( P \left(C_K \mid  \boldsymbol{x} \right) \right)^{1 - \sum_{k=1}^{K-1} t_k} = P \left(C_K \mid  \boldsymbol{x} \right) e^{\sum_{k=1}^{K} t_k u_k} \tag{1}
\end{equation}
$$

を考える。

ただし、$$u_k$$は対数オッズ比：

$$\begin{equation}
u_k= \log \dfrac{P \left(C_k \mid  \boldsymbol{x} \right)}{P \left(C_K \mid  \boldsymbol{x} \right)}
\end{equation} \tag{2}
$$


　この時、$$P\left(y \mid  \boldsymbol{x} \right)$$は教師データ$$\boldsymbol{t}=\left(0 \dots 1_{k'} \dots 0 \right)^{\top}$$によって与えられる確率$$P \left(C_{k'} \mid  \boldsymbol{x} \right)$$を示す。理由は以下の通り。

まず、式(1)の最後の式における右肩は：

$$
\begin{equation}
t_k u_k = \delta_{kk'} \log \dfrac{P \left(C_{k'} \mid  \boldsymbol{x} \right)}{P \left(C_K \mid  \boldsymbol{x} \right)}
\end{equation}
（ただし、 \delta_{kk'} はクロネッカーのデルタ）
$$

より、

$$
\begin{equation}
\sum_{k=1}^{K} t_k u_k = \log \dfrac{P \left(C_{k'} \mid  \boldsymbol{x} \right)}{P \left(C_K \mid  \boldsymbol{x} \right)}
\end{equation}
$$

よって、式(1)の最後の式は

$$
\begin{equation}
P \left(C_K \mid  \boldsymbol{x} \right) e^{\sum_{k=1}^{K} t_k u_k} = P \left(C_K \mid  \boldsymbol{x} \right) e^{\log \frac{P \left(C_{k'} \mid  \boldsymbol{x} \right)}{P \left(C_K \mid  \boldsymbol{x} \right)}} = P \left(C_{k'} \mid  \boldsymbol{x} \right)
\end{equation}
$$

と、確かに確率$$P \left(C_{k'} \mid  \boldsymbol{x} \right)$$を示していることがわかる。<br>
<br>
　更に、対数オッズ比(2)について：
$$
(2) \Leftrightarrow P \left(C_K \mid  \boldsymbol{x} \right)e^{u_k} = P \left(C_k \mid \boldsymbol{x} \right) \tag{2'}
$$

両辺で$$k$$について和を取ると、全確率の法則から：

$$
\sum_{k=1}^{K} P \left(C_K \mid  \boldsymbol{x} \right)e^{u_k} = 1
\Rightarrow P \left(C_K \mid  \boldsymbol{x} \right) = \dfrac{1}{\sum_{k=1}^{K} e^{u_k}}
$$

これを再び対数オッズ比の式(2')に代入すると、

$$
P \left(C_k \mid \boldsymbol{x} \right) = \dfrac{e^{u_k}}{\sum_{j=1}^{K} e^{u_j}}
$$

これを利用して各クラスの確率を求める関数：

$$
\rm{softmax}_k \left(\boldsymbol{u} \right) = \dfrac{e^{u_k}}{\sum_{j=1}^{K} e^{u_j}}
$$

がsoftmax関数である。

## Why use cross entropy error?

対数オッズ比を線形関数でモデル化：

$$
u_k = \boldsymbol{\theta}_k^{\top} \boldsymbol{x}
$$

したものを考えれば、マルチヌーイ分布(1)に対し$$N$$個の入力$$\boldsymbol{x}_1,\dots,\boldsymbol{x}_n$$と教師データ$$\boldsymbol{t}_1,\dots,\boldsymbol{t}_n$$を利用して、パラメータ$$\boldsymbol{\theta}$$について最尤推定を行うことで、予測の確率分布を真の確率分布に近づけることができる。<br>
　すなわち、尤度関数：

$$
L(\boldsymbol{\theta}) = \prod_{n=1}^{N} \prod_{k=1}^{K} \left( P  \left(C_k \mid  \boldsymbol{x}_n \right) \right)^{t_{nk}}
$$

を最大化すればよく、これは交差エントロピー：

$$
E(\boldsymbol{\theta}) = - \sum_{n=1}^{N} \sum_{k=1}^{K}　t_{nk} \log \left( P  \left(C_k \mid  \boldsymbol{x}_n \right) \right) \tag{3}
$$

を最小化することと同義である。故に、交差エントロピー誤差が使用されるのである。

### Meaning of Cross Entropy Error

交差エントロピー誤差は予測の確率分布$$Q(x)$$と真の確率分布$$P(x)$$との距離を求めている。<br>
確率分布の平均情報量(Shannon entropy)：

$$
H(P) = - \sum_x P(x) \log P(x)
$$

を利用して、予測の確率分布$$Q(x)$$から真の確率分布$$P(x)$$への距離（分布の近さ）をKL情報量(Kullback-Leibler divergence)：

$$
D_{KL}(P \mid\mid Q) = \sum_{x \sim P (\rm{x})} P(x) \left( \log P(x) - \log Q(x) \right)
$$

で計算することができる。

　これは、

$$
D_{KL}(P \mid\mid Q) = \sum_{x \sim P (\rm{x})} P(x) \log P(x) - \sum_{x \sim P (\rm{x})} P(x) \log Q(x)
$$

となるが、第1項は真の確率分布のみで構成されており、パラメータによる変化がないから、KL情報量を最小化することは第2項：

$$
H(P,Q) = - \sum_{x \sim P (\rm{x})} P(x) \log Q(x)
$$

を最小化することと同義である。これが交差エントロピーであり、実際、予測の確率をsoftmaxから得られる予測確率($$Q(x)=y_k$$)、真の確率を教師データ($$P(x)=t_k$$)と見なせば：

$$
H(P,Q) = - \sum_{k} t_k \log y_k
$$

と、(3)式と一致することが確かめられる($$y_k = P\left(C_k \mid \boldsymbol{x} \right)$$であるから)。

　すなわち、交差エントロピーを最小化することは、KL情報量の下で与えられる2つの確率分布の近さを最小化することと同義である。



## Forward
### Softmax
**input:** $$ \boldsymbol{u}^{(L)} = \left( u^{(L)}_j \right) $$

**formula:**

$$
y_k = \dfrac{\exp(u_k)}{\sum_{j} \exp(u_j)}
$$


**output:** $$ \boldsymbol{y} = \left( y_k \right) $$

### Cross Entropy
**input:** $$ \boldsymbol{y} = \left( y_k \right), \boldsymbol{t} = \left( t_k \right) $$

**formula:**

$$
E = - \sum_k t_k \log y_k
$$


**output:** $$ E $$



## Backward
**input:** $$ \boldsymbol{y} = \left( y_k \right), \boldsymbol{t} = \left( t_k \right) $$

**formula:**

$$
\dfrac{\partial E}{\partial u_j^{(L)}} = - \sum_k \dfrac{\partial E}{\partial y_k}\dfrac{\partial y_k}{\partial u_j^{(L)}} = - \sum_k \dfrac{t_k}{y_k}(\delta_{kj} y_k - y_j y_k) = - \sum_k t_k (\delta_{kj} - y_j)
$$

ただし、上の$$\delta_{kj}$$はクロネッカーのデルタ。これをまとめると以下になる。

$$
\delta_j^{(L)} = y_j - t_j
$$


**output:** $$ \boldsymbol{\delta}^{(L)} = \left( \delta^{(L)}_j \right) = \left( \dfrac{\partial E}{\partial u_j^{(L)}} \right)$$

## Tensorflow

```python
import tensorflow as tf

# softmaxの実装logits = tf.layers.dense(    inputs=z, # z.shape = (column(i))    units=K    )predictions = {    "classes": tf.argmax(input=logits, axis=1),    "probabilities": tf.nn.softmax(logits, name="softmax_tensor")}# cross entropy errorの実装loss = tf.losses.sparse_softmax_cross_entropy(labels=labels, logits=logits)
```
