---
layout: post
title: "記事の書き方"
date: 2018-03-04
---

# とりあえず…
_postフォルダ内にある.mdファイル見れば書き方大体察せる

# YAML Front Matter
上の"---"で囲まれた部分はYAML Front Matterと言って、記事では実際に表示されない部分。記事の最初に記述する。基本的には以下を記述。

- layout: _layoutフォルダ内にあるファイルを指定(例：post)
- title: titleをダブルクオーテーションで囲んで記述(例："アランが生まれました✌️")
- category: カテゴリを記載(例："document")
- tags: リスト表記でタグを記載(例：["birth", "yeah"])
- date: 編集日を例にならって記載(例：1912-06-23)
- author: ユーザ名を記載(例：Ethel Sara Turing)


# markdown編集用のアプリ

- Mac: [MacDown](https://macdown.uranusjr.com/)
	- システム環境設定→一般でプレビューを自動的に更新をoff
		- これやらないとフリーズする
		- 更新はcommand+R
	- システム環境設定→レンダリングでLaTeX風の数式シンタックスをon

# markdownの書き方

[ここ](https://qiita.com/tbpgr/items/989c6badefff69377da7)見りゃだいたいわかる

## 補足

### 数式
$$\LaTeX$$使って書ける。ドルマーク2つで開始と終了を宣言

$$
\boldsymbol{u}_j = \sum_i \boldsymbol{W}_ji \boldsymbol{z}_i + \boldsymbol{b}_j
$$

同じ行にも$$y=f(x)$$入れられる👍<br>
$$\LaTeX$$の書き方はググるのだー<br>
ちなみに、[ここ](https://webdemo.myscript.com/views/math.html#/demo/equation)なら手書き数式を$$\LaTeX$$に変換してくれる



### 画像
サイズ調整できるhtmlタグを使ったほうがいい
```
<img src="https://huitclub.github.io/images/ここにファイル名.jpg" alt="Figure1" title="ファイル名等" height="高さ" width="幅">
```

### ソースコード
バッククオート(Macならshift+@)で囲うより、

```
{% highlight language linenos %}
def hello():
	print("Hello")
{% endhighlight %}
```

ってやると、行番号が振られる。ただし、この場合、macdownには正しく表示されない（表示される方法あるかも...？）

# Jekyllについて
[ドットインストール](https://dotinstall.com/lessons/basic_jekyll)見れば概要はわかる。これをGithub Pagesでビルドしてる。