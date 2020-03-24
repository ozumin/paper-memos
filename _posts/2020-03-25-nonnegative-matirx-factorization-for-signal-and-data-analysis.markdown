---
layout: post
title:  "Nonnegative Matrix Factorization for Signal and Data Analytics: [Identifiability, Algorithms, and Applications] (Fu et al., 2018)"
date:   2020-03-25 00:07:13 +0900
categories: paper
---
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  tex2jax: {
    inlineMath: [['$','$'], ['\\(','\\)']],
    processEscapes: true
  }
});
</script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
[論文](https://arxiv.org/abs/1803.01257)
# TL;DR
NMFの理論と応用についてと，NMFのidentifiabilityについて．
いくつかのidentification criteriaの紹介とその利点欠点．

# Introduction
<font color="Green">不勉強なのでちょっと詳しめに</font>

最初はpositive matrix factorization (PMF)という名前で出てきた．
Analytical chemistryとremote sensing (earth science)の研究者は1990年代から使えると気づいていた．
1999年のLeeとSeungの論文で一気に注目を浴びるようになった．
#### Interpretability (解釈可能性) and model identifiability
NMFの解釈のしやすさが解析における人気の理由．
自然画像やハイパースペクトラル画像，文書，community detetectionの分解など幅広く使われる．
データが生成モデルに従っているならば，真の解を知りたい．
NMFのidentifiabilityに対する理解を深めることで，話者分離やイメージングの分野での応用に役立つ．
NMFはLSA,LDA,MMSB,HMMから生成されるトピックモデルを抽出できるとされている．

# Where does NMF arise?
#### Early pioneers
始まりは1994年のhyperspectral unmixing (HU)の論文で，この時はPMFだった．
一番知られているのはLeeとSeungの画像認識の論文．

この後はいろんな分野での応用例の紹介．
Identification criteriaと計算ツールはどれを使えばいいかは，identifiabilityの問題に関わる．
# NMF and model identifiability
#### Problem statement
行列分解のidentifiabilityの問題は以下である．

$$
\bf X = WH^T = WQ(HQ^{-T})^{-T}
$$

この$\bf Q$が無限に存在するのが問題．
NMFでは非負制約が入っているので$\bf Q$の解空間のvolumeが狭まる．
ここでは，行と列の順番がバラバラになるのやスケールが違ってくるのはidentifiabilityの問題としては捉えない（応用する時に問題にはなるが）．
その上でidentifiabilityのある状況を定義している．

データが$\bf X = W_\sharp H_\sharp^T$のモデルに従い，$\bf W_* H_*$ が最適解の時，

$$
\bf W_* = W_\sharp \Pi D, \ H_* = H_\sharp \Pi D^{-1}
$$

が成り立つとidentifiable．
ただし，$\bf \Pi$が置換行列<font color="Green">（置換行列は，各行各列にちょうど一つだけ1の要素を持ち、それ以外は全て0の行列）</font>で，$\bf D$がフルランク<font color="Green">（フルランクの時逆行列が存在する）</font>の対角行列．
<font color="Green">よくわからない</font>

NMFは適切なidentification criterionを用いればidentifiableである．

#### Preliminaries
NMFの幾何的な解釈．
データ$\bf X$の行は$\bf W$の列のconic hullに属している．

また，$\bf H$の列和が1という仮定はrow-stochastic assumptionといい，とても使いやすくなる．
実際に使う時にこの仮定を置けない場合は$\bf X$の列和を1にすると無理やり使える．
この時，データの行は$\bf W$の列のconvex hullに属している．
この2つを表した図が以下．
![Cone](/assets/2020-03-25-1.png)

Separabilityとsufficiently scatteredという条件がある．
<font color="Green">ここもよくわからない．．</font>

#### Separability-based methods for NMF
Separability条件をもち，identifiabilityが保証されている手法の紹介．
Separabilityがあるということはconvex hullがデータにtouchedだということ．
![separability](/assets/2020-03-25-2.png)
<font color="Green">そうか，$\bf H_\sharp$のある列が単位ベクトルということは，それが$\bf W$の行にかけるからその行がそのままデータ$\bf X$の行になるのか</font>
つまり，$\Lambda$をインデックスだとすると，${\bf X}(:,\Lambda) = {\bf W_\sharp}$となる（ノイズがない場合），
なので問題は，全てのデータ点を含むconvex hullを張るデータ点を探すことになる．
<font color="Green">なるほど...?</font>

このようなデータ点をvertices (convex hull)かextreme rays (conic hull)というらしい．
性質から式(14)が成り立つらしい．
<font color="Green">なんとなくわかるような</font>
これを解くにはNが大きいと大変だよと．

大量の凸最適化を避けるのにconvex optimazation-based NMFが提案された（式(15)）．
Block-sparse optimaizationのフレームワークらしい．
このままでは凸ではないので条件を緩和して凸にしたのが式(16)．
ただ，計算コストが高いので，次の貪慾法を使ってからこの手法を使うのが良いのでは，と書いてある．
