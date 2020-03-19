---
layout: post
title:  "A Double Parametric Bootstrap Test for Topic Models (Seto et al., 2017)"
date:   2020-03-18 01:07:13 +0900
categories: paper
---
# TL;DR
データの生成分布がNMFの仮定に合っているかを確かめる方法としてDouble Parametric Bootstrap Testが使えるよという話．

# 背景・問題
NMFで仮定している分布にデータが沿っているかを確認したい．
関連研究は[Bayesian checking for topic models (Mimno and Blei, 2011)](https://dl.acm.org/doi/10.5555/2145432.2145459)．
この研究では，LDAの独立性の仮定が満たされているかをbayesian posterior predictive checks <font color="Green">(ベイズ事後分布をみること?)</font>によって検証した．
<font color="Green">事後分布はデータが与えられた時にパラメタがどのような分布を取るか</font>

# 手法
KL divergenceの最小化とPoisson分布の最尤推定のduality(双対性)について書かれていた．
<font color="Green">ポアソン分布に従うデータのNMFはI divergence (一般化KL divergence)と同じ．同様に正規分布に従うデータのNMFはFrobenius normと同じ．
ここでいうdualityとは，同じ目的関数を使うよってこと?</font>

[Double paramtetric bootstrap test (Beran, 1987)](https://www.jstor.org/stable/2336685?seq=1)が今回使える．
<font color="Green">Double bootstrap testとは，ブートストラップサンプルからさらにブートストラップするということみたい．parametricが何なのかはよくわからない．</font>
普通のブートストラップはサンプル数が大きくなるに連れて精度がよくなるが，サンプル数が有限だとよくないことがある．
真のp値が未知の分布に従っているのに対し，ブートストラップのp値はブートストラップの分布に従っており，この分布が以下の場合に異なってしまう：
- test statistic (検定統計量)がpivotal(モデルのパラメタによらず分布が同じな確率変数)ではない時
- ブートストラップを作る際のパラメタの真値と推定値が異なる時

検定統計量が漸近的にpivotalな場合はサンプル数が大きくなるに連れてdouble bootstrap distributionが真の分布に収束する．
Double bootstrapのp値がasymptoticなp値<font color="Green">(通常のブートストラップのってこと?)</font>よりも早く真のp値に収束することを[Beran, 1988](https://www.jstor.org/stable/2289292?seq=1)が示した．

用いるアルゴリズムが載っている．
この代替となるのは残差型ブートストラップ．
しかし今回のデータはカウントした整数の行列なので，heteroscedasticity (分散不均一性)をもつ．
Heteroscedasticityをもつ線形モデルようにwild bootstrap (ランダムに残差に重み付けするブートストラップ)もあるが...<font color="Green">これがなぜダメなのかよくわからない．</font>
正規分布であれば分散は一定で残差の分散も一定になるが，ポアソン分布の残差の分散は不均一なので，残差型よりdouble parametricの方が良い．

# 実験
ガンマ分布から2つの行列を生成して，ポアソン分布を仮定したNMFで分解した．
p値の分布が一様分布と同じかどうかをKolmogorov-Smirnov検定(KS検定)で検定した．
<font color="Green">ここらへんでp値について復習したくなった．</font>
