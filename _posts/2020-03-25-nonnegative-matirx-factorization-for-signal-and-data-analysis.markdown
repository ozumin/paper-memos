---
layout: post
title:  "Nonnegative Matrix Factorization for Signal and Data Analytics: [Identifiability, Algorithms, and Applications] (Fu et al., 2018)"
date:   2020-04-13 00:07:13 +0900
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

他にもいろいろ方法はあって，例えばellipsoid volume minimization-based NMF.
これは$\bf x_1,\dots, x_N$と$\bf -x_1,\dots, -x_N$を含むminimum-volume ellipsoidを探す．
上のアルゴリズムよりは計算コストは低いが$R$についての知識がないと無理．

ここからgreedy algorithmの話．

Greedy algorithmでは，まず頂点を探す．
探した方は：

$$
\hat{l}_1 = \text{arg max}_{l \in \{1,\dots,N\}} ||{\bf x}_l||_q
$$

で，これは簡単に証明できる．
<font color="Green">（証明によるとこの頂点は単位ベクトルになるのか...？）</font>
最初の頂点が見つかったらそのベクトルにデータを射影して，次の頂点を探す...を繰り返すらしい．
いろんな名前のアルゴリズムがあるが，一番使われているんはsuccessive projection algorithm (SPA)らしい．
このアルゴリズムは計算コストが低く$q=1$以外はノイズに頑健．
しかし，間違うとそれが引き継がれていってしまう．

#### Separability-free methods for NMF
Separability-basedな方法は$\bf H_\sharp$がrow-stochasticであれば$\bf W_\sharp$が非負だと仮定せずに使える．
$\bf H_\sharp$が式(11)を満たさない場合は$\bf X$の列を正規化しなければならず，その場合$\bf W_\sharp \geq 0$の仮定は必要になる．
Separability-based NMFはよく研究されていて，ノイズがある場合も研究されている．
しかし，separabilityの仮定は頑健性の面で脆くする．
例えば，hyperspectral unmixingの話だと，ピクセルに1つの物質しかないpure pixelがデータに存在するということである．
テキストマイニングでいうと，全てのトピックは他のトピックでは使われない単語が必ず存在するということ．
これは現実問題では満たされないものかもしれない．
では，separabilityがない場合に$\bf W_\flat$と$\bf H_\flat$のidentifiabilityは保証されるのか？
それが次に述べられている．

一番人気のあるNMFのcriterion式(10)に帰る：

$$
minimize_{\bf W \geq 0, \ \bf H \geq 0} ||{\bf X - WH^T}||_F^2,
$$

これからこれをplain NMF criterionと呼ぶ．
このcriterionのidentifiabilityを調べるには以下の問題のidentifiabilityを調べれば良い(?)：

$$
\text{find } {\bf W, \ H} \\
\text{subject to } {\bf X = WH^T} \\
{\bf W} \geq 0, \ {\bf H} \geq 0
$$

この問題のidentifiabilityがある十分条件は
1）$\bf H_\flat$はseparability条件を満たす，
2）$\bf W_\flat$にはある0のパターンがある．
2つ目については例えば，$\bf W_\flat$の列全てに0が入っていて，その0の位置は行ごとに異なる<font color="Green">（全く同じ場所に0がある列はないってこと？）</font>
これは$\bf W_\flat(m,:)$が全ての$\mathcal{R}_+^R$の面に接していることを保証している？
他にもこのような条件を設定している論文がある．

このidentifiabilityがある条件はこの問題を見ることで理解できる．
$\bf X = W_\flat Q (H_\flat Q^{-T})^T$が成り立つ$\bf Q$があるとする．
<font color="Green">$\bf Q^{-T}$ってナンだ</font>

![Q](/assets/2020-03-25-3.png)

$\bf Q^{-1}$はオレンジの領域を回転，拡大，縮小するオペレータと考えることができる．
もし$\bf H_flat$がseparableかsufficiently scatteredで，$\bf H_\flat Q^{-T}$が非負だという制約がある時，$\bf Q^{-T}$は回転や拡大はできない．
そして，縮小もできない．
なぜなら，図の緑の領域を拡大することになるから．
<font color="Green">なぜ拡大しちゃいけないんだろう．</font>
もし$\bf W_\flat$がsufficiently scatteredかいくつかの行が非負象限の境界に接している<font color="Green">(いくつか0が含まれてるってことかな)</font>なら，$\bf  W_\flat Q$はinfeasible(実行不可能?)である．
これより，$\bf Q$は置換行列で列をスケールする行列となる．

Rが大きくなるに連れてsufficiently scatttered条件のCは縮んでいくので，いろんな場面でsufficiently scattered条件は満たされると思われる．
sufficiently scatteredはplain NMF criterionやそれ以外のcriterionを作るのに必要だった．

##### Is the sufficiently scattered condition easy to satisfy?
Sufficiently scattered条件を満たしているかを確かめるのはNP困難．
1つの方法としては式(20)で確かめること．
この方法で筆者はランクの$\bf H$の非負要素の割合を変化させて実験を行った．
非負要素の割合が増えるとsufficiently scattered条件は満たされないという関係があったらしい．

次にsimplex volume minimization (VolMin)の話．
Plain NMFのidentifiabilityには$\bf H$と$\bf W$が0を含むことが必要だったが，それを満たせないモデル（hyperspectral unmixingとか）がある．
その時にどうすればいいのかという話．

データが$conv\{\bf W\}$の中にsufficiently spreadなら，$\{x_1,\dots, x_N\}$を含めるsimplexの体積を最小にすることで$\bf W_\flat$がもとまる．
これはCraigが1994年に提唱したことで，2015年にはFuなどが次のようなVolMin問題を作成した：

$$
\text{minimize}_{\bf W, H} \rm{det}({\bf W^T W}) \\
\text{subject to } \bf X = WH^T \\
\bf H \geq 0, \ 1^T H^T = 1^T,
$$

そして理論的な証明もなされた．
VolMinは$\bf H_\flat$の行が散らばるのに関与しており，それは$\bf W_\flat(:,1),\dots,W_\flat(:,R)$で貼られるconvex hullの中に$\bf x_1,\dots,x_N$が散らばることを意味する．
Convex hullの中をデータが散らばるようになることはvolumeが小さくなるのと同じ（それはなんとなくわかる）．
Plain NMFではidentifiabilityがないような問題でもこのVolMinだと大丈夫．
しかし，デターミナントがあるので最適化は大変．

VolMin NMFはplain NMFやseparability-based NMFよりも条件が緩くなった．
しかし，$\bf H_\flat$がrow-stochasticという条件があった．
それを一般化するためにVolMinの$\bf H$の制約を変化させたのが式(22)．
これによって，NMFはfull column rank $\bf W_\flat$とsufficiently scattered nonnegative $\bf H_\flat$であればidentifiableであるとなった．

<font color="Green">3つの人工データで4つのアルゴリズムを比較しているが，$\bf W$が非負じゃない時（負の値が含まれている場合）もやっていて，NMFとは...?となった</font>

#### Algorithms

Separatbility-based NMFについてはもうよく研究されている．
Plain NMFについてはMUなどでちゃんと解かない方法やちゃんと解くBCDアルゴリズムなどがある．
前者はシンプルだがイテレーションが多く，後者は複雑だがイテレーションは少ない．

あとはVolMinのデターミナントの最小化について書かれている．
Successive Convex Approximation (SCA)は$\bf W$がフルランクの平方行列だとすると，$det(\bf{W^TW})= |det({\bf W})|^2$なので最小化するのは$|det(\bf{W})|$にしたもの．
Alternating Linear Programming (ALP)は行ごとに更新していく方法．
Regularized Fittingとかもある．

#### More discussions on applicatonsは飛ばす
#### Take home points, open questions, concluding remarks
ちゃんと合ったNMFを使いましょう．
Plain NMFはhyperspectral unmixingとかには向かないね．
Separability-based NMFは都合の良い特徴がたくさんあってcommunity detectionやhyperspectral unmixingとかに使える．
Separabilityがあるようなデータではこれを最初に試すのが良いのでは．
Sufficiently scattered condition-based NMFではtopic modelingやHMM identificationなどでseparability-based NMFより良い結果となる．
しかし，最適化が難しい．

他にもNMFがノイズにどれくらいロバストかはよくわかってなかったり，identifiabilityに必要な条件はよくわかってなかったりということが書かれている．
