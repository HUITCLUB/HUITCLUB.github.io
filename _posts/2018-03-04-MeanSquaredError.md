---
layout: post
title: "Mean Squared Error （二乗和誤差）"
date: 2018-03-04
category: 
---

## Model Structure
<img src="https://huitclub.github.io/images/mse.jpg" alt="Figure1" title="MSE" height="300">

　二乗和誤差は回帰問題に使用される誤差関数。ニューラルネットワークの出力$$\boldsymbol{y}$$を受け取り、教師データ$$\boldsymbol{t}$$を利用して誤差を計算する。

## Why use MSE?
　二乗和誤差は、統計学における最小二乗法に基づいて与えられている。


### 線形モデル
　まず、線形モデル：

$$
\boldsymbol{y} = \boldsymbol{X \theta} + \boldsymbol{\varepsilon}
$$

$$
y_i = \sum_j x_{ij} \theta_j + \varepsilon_i
$$

を考える。ここで、各変数は

- $$\boldsymbol{y} =\left(y_{i}\right)$$: $$i$$番目の入力$$\boldsymbol{x}_i$$に基づく、誤差$$\varepsilon_{i}$$込みの実測値
- $$\boldsymbol{X} =\left(x_{ij}\right)$$: $$j$$番目のパラメータ$$ \theta_{j}$$に対応する$$i$$番目の入力値
- $$\boldsymbol{\theta} =\left( \theta_{j}\right)$$: 線形モデルのパラメータ
- $$\boldsymbol{\varepsilon} = \left( \varepsilon_{i}\right)$$: $$i$$番目の誤差 $$\sim \mathcal{N} \left( 0, \sigma^2 \right)$$

であり、誤差の確率分布から$$\boldsymbol{y} \sim \mathcal{N} \left( \boldsymbol{X \theta}, \sigma^2 \right)$$


### 最良推定不偏推定量(Best Linear Unbiased Estimator; BLUE)

$$\boldsymbol{y}$$についてのBLUEを$$\boldsymbol{y}_{\rm{BLUE}}$$と書く。

性質

- $$E\left(\boldsymbol{y}_{\rm{BLUE}}\right) = \sum_j x_{ij} \theta_j $$ すなわち、誤差項のない真の値
- $$V\left(\boldsymbol{y}_{\rm{BLUE}}\right) = \min  V\left(\boldsymbol{y}\right)$$ 分散を最小にする
- $$Cov\left(\boldsymbol{y}_{\rm{BLUE}}\right) = 0$$ 独立


例：無作為抽出$$X_1,\dots,X_n \sim \mathcal{N}\left(\mu, \sigma^2\right)$$及び実数$$c_1,\dots,c_n$$に対し、推定量$$T = c_1 X_1 + \dots + c_n X_n $$を定義する。$$T$$がBLUEになる時、$$c_1 = \dots = c_n = \frac{1}{n}$$であり、$$T = \frac{1}{n} \sum_i X_i = \bar{X}$$である。

### 最小二乗法の原理
　一般の線形モデル$$\boldsymbol{y} = \boldsymbol{X \theta}$$において、未知母数$$\boldsymbol{\theta}$$のある係数$$\boldsymbol{l}$$（$$\boldsymbol{l}$$は$$\boldsymbol{X}$$の各行ベクトルに対応）による線形結合：

$$\boldsymbol{l}^{\top}\boldsymbol{\theta} = \sum_j l_j \theta_j $$

についてのBLUEを考える。逐次$$\boldsymbol{l}$$に対応するBLUEを求めるのは煩雑であるため、二乗和誤差：

$$
S \left( \boldsymbol{\theta} \right) = \left\| \boldsymbol{y}-\boldsymbol{X\theta} \right\|^{2}= \left( \boldsymbol{y}-\boldsymbol{X\theta} \right)^{\top} \left( \boldsymbol{y}-\boldsymbol{X\theta} \right)
$$

を最小にする$$\boldsymbol{\theta} = \widehat{\boldsymbol{\theta}}$$を求めて$$\boldsymbol{l}^{\top} \widehat{\boldsymbol{\theta}}$$をBLUEとすることが、最小二乗法の原理である。

　これは、二乗和であることから、$$\nabla_{\boldsymbol{\theta}} \left. S  \left( \boldsymbol{\theta} \right) \right|_\widehat{\boldsymbol{\theta}} = \boldsymbol{o} \Leftrightarrow \boldsymbol{\theta} = \arg \min_{\boldsymbol{\theta}} S\left( \boldsymbol{\theta} \right)$$であるが、後述のガウス・マルコフの定理により、$$\nabla_{\boldsymbol{\theta}} \left. S  \left( \boldsymbol{\theta} \right) \right|_\widehat{\boldsymbol{\theta}} = \boldsymbol{o}$$となる$$\widehat{\boldsymbol{\theta}}$$がBLUEになることが保証されている。よって、二乗和である$$S  \left( \boldsymbol{\theta} \right)$$を最小化することでBLUEを求められるのである。
　
### 正規方程式
$$\nabla_{\boldsymbol{\theta}} \left. S  \left( \boldsymbol{\theta} \right) \right|_\widehat{\boldsymbol{\theta}} = \boldsymbol{o}$$となるとき、$$S  \left( \boldsymbol{\theta} \right)$$の形から、

$$
\dfrac{\partial S}{\partial \theta_j} = -2 \sum_i x_{ij} \left( y_i - \sum_{j'} x_{ij'} \theta_{j'} \right) = 0
$$

となるので、

$$
\sum_i x_{ij} \left(\sum_{j'} x_{ij'} \theta_{j'} \right) = \sum_i x_{ij} y_i
$$

これを行列形式にすると、

$$
\boldsymbol{X}^{\top} \boldsymbol{X \theta}=\boldsymbol{X}^{\top} \boldsymbol{y}
$$

となり、これを正規方程式という。これを$$\boldsymbol{\theta}$$について解けば、$$\widehat{\boldsymbol{\theta}}$$を求められる。

### ガウス・マルコフの定理
　線形モデルに関する任意の推定可能関数$$\boldsymbol{l}^{\top}\boldsymbol{\theta}$$について、正規方程式$$
\boldsymbol{X}^{\top} \boldsymbol{X \theta}=\boldsymbol{X}^{\top} \boldsymbol{y}
$$を満たす最小二乗解$$\boldsymbol{\theta}=\widehat{\boldsymbol{\theta}}$$から得られる$$\boldsymbol{l}^{\top}\widehat{\boldsymbol{\theta}}$$が一意にBLUEを与える。

#### 簡易的な証明
ここでは$$\boldsymbol{X}$$がフルランク($$\rm{rank} \left( \boldsymbol{X} \right) = \dim \boldsymbol{\theta}$$)の時のみ証明する。また、フルランク時以外でも[擬似逆行列](https://ja.wikipedia.org/?curid=427079)を用いることで、証明できる。

**証明**

（線形不偏推定量の証明）

$$
\boldsymbol{X}^{\top} \boldsymbol{X}\widehat{\boldsymbol{\theta}}=\boldsymbol{X}^{\top} \boldsymbol{y}
\Leftrightarrow \widehat{\boldsymbol{\theta}}=\left( \boldsymbol{X}^{\top} \boldsymbol{X} \right)^{-1}\boldsymbol{X}^{\top} \boldsymbol{y} \tag{1}
$$

より、$$\forall \boldsymbol{l}^{\top}\boldsymbol{\theta}$$に対し、

$$\boldsymbol{l}^{\top} \widehat{\boldsymbol{\theta}} =\boldsymbol{l}^{\top} \left( \boldsymbol{X}^{\top} \boldsymbol{X} \right)^{-1}\boldsymbol{X}^{\top} \boldsymbol{y}$$

また、期待値：

$$
E \left( \boldsymbol{l}^{\top}\widehat{\boldsymbol{\theta}} \right) = 
\boldsymbol{l}^{\top} \left( \boldsymbol{X}^{\top} \boldsymbol{X} \right)^{-1}\boldsymbol{X}^{\top}  E\left( \boldsymbol{y} \right) = \boldsymbol{l}^{\top} \left( \boldsymbol{X}^{\top} \boldsymbol{X} \right)^{-1}\boldsymbol{X}^{\top} \boldsymbol{X \theta} = \boldsymbol{l}^{\top} \boldsymbol{\theta}$$

より、$$\boldsymbol{l}^{\top}\widehat{\boldsymbol{\theta}}$$は$$\boldsymbol{l}^{\top} \boldsymbol{\theta}$$の線形不偏推定量である。

（線形不偏推定量の証明終）

（最良であることの証明）


　次に、任意の線形不偏推定量$$\boldsymbol{L}^{\top} \boldsymbol{y}$$を考え、差：

$$
\boldsymbol{l}^{\top}\widehat{\boldsymbol{\theta}} - \boldsymbol{L}^{\top} \boldsymbol{y} = \left\{ \boldsymbol{l}^{\top} \left( \boldsymbol{X}^{\top} \boldsymbol{X} \right)^{-1} \boldsymbol{X}^{\top} - \boldsymbol{L}^{\top} \right\} \boldsymbol{y} \equiv \boldsymbol{b}^{\top} \boldsymbol{y}
$$

を取ると、$$\forall \boldsymbol{\theta}$$に対し、$$E \left( \boldsymbol{L}^{\top} \boldsymbol{y} \right) = \boldsymbol{l}^{\top} \boldsymbol{\theta}$$より、$$E \left( \boldsymbol{b}^{\top} \boldsymbol{y} \right) = \boldsymbol{b}^{\top} \boldsymbol{X \theta} \equiv 0$$が成り立つので、

$$
\boldsymbol{b}^{\top} \boldsymbol{X} \equiv \boldsymbol{o} \tag{2}
$$

ここで、$$\boldsymbol{L}^{\top} \boldsymbol{y}$$の分散：
$$V \left( \boldsymbol{L}^{\top} \boldsymbol{y} \right) = V \left( \boldsymbol{l}^{\top} \widehat{\boldsymbol{\theta}} - \boldsymbol{b}^{\top} \boldsymbol{y} \right) = V \left( \boldsymbol{l}^{\top} \widehat{\boldsymbol{\theta}} \right) - 2Cov \left( \boldsymbol{l}^{\top} \left( \boldsymbol{X}^{\top} \boldsymbol{X} \right)^{-1}\boldsymbol{X}^{\top} \boldsymbol{y}, \boldsymbol{b}^{\top} \boldsymbol{y} \right) + V \left( \boldsymbol{b}^{\top} \boldsymbol{y} \right) \tag{3} $$ 

ここで、$$\boldsymbol{y}$$の誤差が$$\boldsymbol{\varepsilon} \sim \mathcal{N} \left( 0, \sigma^2 \right)$$の下で$$\boldsymbol{p}^{\top} \boldsymbol{y} = \sum_i p_i y_i, \boldsymbol{q}^{\top} \boldsymbol{y} = \sum_i q_i y_i$$とした時、共分散は

$$Cov \left( \boldsymbol{p}^{\top} \boldsymbol{y}, \boldsymbol{q}^{\top} \boldsymbol{y} \right) = Cov \left( \boldsymbol{p}^{\top} \boldsymbol{\varepsilon}, \boldsymbol{q}^{\top} \boldsymbol{\varepsilon} \right) = \sum_i p_i q_i \sigma^2 = \boldsymbol{p}^{\top} \boldsymbol{q} \sigma^2$$

となるから式(2)を用いると、式(3)の第2項：

$$
Cov \left( \boldsymbol{l}^{\top} \left( \boldsymbol{X}^{\top} \boldsymbol{X} \right)^{-1}\boldsymbol{X}^{\top} \boldsymbol{y}, \boldsymbol{b}^{\top} \boldsymbol{y} \right) = \boldsymbol{l}^{\top} \left( \boldsymbol{X}^{\top} \boldsymbol{X} \right)^{-1}\boldsymbol{X}^{\top} \boldsymbol{b} \sigma^2 = 0
$$

よって、式(3)から、

$$V \left( \boldsymbol{L}^{\top} \boldsymbol{y} \right) = V \left( \boldsymbol{l}^{\top} \widehat{\boldsymbol{\theta}} \right) + V \left( \boldsymbol{b}^{\top} \boldsymbol{y} \right) \Rightarrow V \left( \boldsymbol{L}^{\top} \boldsymbol{y} \right) \geq V \left( \boldsymbol{l}^{\top} \widehat{\boldsymbol{\theta}} \right) $$

となるので、$$\boldsymbol{l}^{\top} \widehat{\boldsymbol{\theta}}$$はBLUEである。


（最良であることの証明終）

 **証明終**

#### 幾何学的な意味
　式(1)から、
$$\boldsymbol{X} \widehat{\boldsymbol{\theta}}=\boldsymbol{X} \left( \boldsymbol{X}^{\top} \boldsymbol{X} \right)^{-1}\boldsymbol{X}^{\top} \boldsymbol{y} $$を考える。すると、

$$
\Pi_X = \boldsymbol{X} \left( \boldsymbol{X}^{\top} \boldsymbol{X} \right)^{-1}\boldsymbol{X}^{\top}
$$

は、以下の性質を満たす。

1. 対称性 $$ \Pi_X^{\top} = \Pi_X$$
2. 冪等性 $$ \Pi_X^2 = \Pi_X $$

　よって、$$\Pi_X$$は射影子であり、$$\mathbb{R}^m$$空間上の点から、$$\mathbb{R}^m$$上の$$\boldsymbol{y}=\boldsymbol{X \theta}$$び貼る空間への正射影となっており、BLUEであることが直感的に捉えられる。

### Neural Networkへの導入
　まず、線形モデル：

$$
\boldsymbol{y} = \boldsymbol{X \theta} + \boldsymbol{\varepsilon}
$$

$$
y_i = \sum_j x_{ij} \theta_j + \varepsilon_i
$$

と、基本的なNueral Networkの変数の対応は以下のようになる。

| 上記の変数 | 上記での説明 | NNの変数 | NNでの説明 |
|:---|:---|:---|:---|
| $$\boldsymbol{y}$$ | 実測値 | $$\boldsymbol{t}$$ | テストデータ |
| $$\boldsymbol{X}$$ | 入力値 | $$\boldsymbol{x}_i$$ | バイアスを考慮した$$i$$番目の入力データ |
| $$\boldsymbol{\theta}$$ | パラメータ | $$\boldsymbol{w}$$ | 全層での重み及びバイアスを一つにしたパラメータ |
| $$\boldsymbol{X} \widehat{\boldsymbol{\theta}}$$ | BLUE |         $$\boldsymbol{y}$$ | NNによる予測値 |

　最下行に記したように、Neural Networkによる予測値$$\boldsymbol{y}$$をBLUEに一致させることが理想である。よって、二乗和誤差：

$$
\begin{equation}
E \left( \boldsymbol{w} \right) = \sum_n \sum_k \left(t_{nk} - y_{k}\left( \boldsymbol{x}_n ; \boldsymbol{w}\right) \right)^2
\end{equation}
$$

を最小化させれば良い。

　これをオンライン学習に対応させ、微分を簡単にしたものが、Neural Networkでの二乗和誤差：


$$
\begin{equation}
E \left( \boldsymbol{w} \right) = \dfrac{1}{2} \sum_k \left(t_{k} - y_{k}\left( \boldsymbol{x} ; \boldsymbol{w}\right) \right)^2
\end{equation}
$$

である。


## Forward

**input:** $$ \boldsymbol{y} = \left( y_k \right), \boldsymbol{t} = \left( t_k \right) $$

**formula:**

$$
\begin{equation}
E  = \dfrac{1}{2} \sum_k \left(t_{k} - y_{k}\left( \boldsymbol{x} ; \boldsymbol{w}\right) \right)^2
\end{equation}
$$


**output:** $$ E $$



## Backward
**input:** $$ \boldsymbol{y} = \left( y_k \right), \boldsymbol{t} = \left( t_k \right) $$

**formula:**

$$
\dfrac{\partial E}{\partial y_k} =  y_k - t_k
$$

**output:** $$ \nabla_{\boldsymbol{y}} E = \left( \dfrac{\partial E}{\partial y_k} \right)$$

## Tensorflow

```python
import tensorflow as tf

# lossの計算
loss = tf.reduce_mean(tf.square(t - y))

# lossをOptimizer(例：勾配降下法)を指定して最小化
train_step = tf.train.GradientDescentOptimizer(µ).minimize(loss)

```
