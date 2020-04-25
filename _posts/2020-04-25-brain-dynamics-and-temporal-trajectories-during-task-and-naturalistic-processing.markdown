---
layout: post
title:  "Brain dynamics and temporal trajectories during task and naturalistic processing (Manasij et al., 2019)"
date:   2020-04-25 01:07:13 +0900
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
[論文](https://www.sciencedirect.com/science/article/abs/pii/S105381191832086X)

前まとめたやつを持ってきただけ．
## TL;DR
Reservoir computingを使ってfMRIデータを解析したという話．
- タスクをかけた時のタスクのclassificationで良い精度がでた
- RNNの出力のトラジェクトリーを書いたらタスクによって違った

## Introduction
fMRI: 神経が活動すると血流が上がる．そのBOLD信号を計測する．時間分解能は1-2sと低いが，空間分解能が高い
今までのfMRIデータの解析としては静的なデータにしていた（区間で平均の値を解析するなど）
→時間方向の情報もみたい

Recurrent neural networks(RNNs)は時系列の解析に注目を集めているが，学習には大量のデータが必要

Reservoir computingはRNNの出力層のみを学習させる手法．
Echo-state networks (Jaegar, 2001)とliquid-state machines (Maass, 2002)が元になっている（それぞれの違いは，tanhとspiking neural network）
時系列予測やtemporal signal classificationなどに使われている．

この論文でやったのは
1. fMRIデータのclassification．タスクの違うデータを分類できるかどうか
2. characterize the dimentionality of the temporal information useful for classification．高次元でノイズの乗ったデータから低次元のトラジェクトリを抽出する

これがFig 1.
![](https://i.imgur.com/Zn0PsMZ.jpg)

## Methods
### データについて
1. Working memory dataset
    200人のデータ(100人はテスト用)
    提示された画像が2個前のものと一致するかどうかというタスク，コントロールはただ見せられただけ？
2. Theory of mind dataset
    200人のデータ
    動画がsocial interactionを含むかどうかを答えるタスク
4. Movie data
    12人のデータ
    怖いか面白いかneutralな動画を見せた

fMRIデータそのままだと次元数が多いのでROIが設定された．
k-meansクラスタリングで500ROIになった．
TR: 1.25s

### Reservoir computing
Echo-state networkのformulationを使った．
${\bf u}(t)$: 入力
${\bf z}(t)$: 出力
${\bf x}(t)$: reservoir state
$\tilde{\bf x}(t)$: 発火みたいな感じ？

$$
\tilde{\bf x}(t) = {\rm tanh} ({\bf W}^{in}[1;{\bf u}(t)] + {\bf Wx}(t-1)) \\
{\bf x}(t) = (1-a){\bf x}(t-1) + \alpha \tilde {\bf x}(t)
$$

$\alpha$: leakage rate．どれくらいで忘れるか

${\bf N_u}$: 入力の大きさ
${\bf \tau}$: パラメタ
${\bf N_x} = {\bf \tau} \times {\bf N_u}$: reservoirの大きさ．$\bf \tau$時刻前の情報を覚えておきたかったら最低この大きさにしなければならないらしい[(Lukoševičius, 2012)](https://link.springer.com/chapter/10.1007/978-3-642-35289-8_36)

$\bf W$: 重み．大きさは$\bf N_x \times N_x$．スパースで要素は$\mathcal{N}(0,1)$からサンプルされる．$10 {\bf N_x}$個の要素が0でうまる．
$\bf W^{in}$: 入力の重み．要素は同じ分布からサンプルされるがスパースではなく密．

Reservoirでは$\bf W^{in}$と$\bf W$は学習されず，$\bf W^{out}$のみ学習される．
Echo state propertyを保証するために$\bf W$の最大固有値が1以下であることが重要．[(Jaeger, 2001)](https://pdfs.semanticscholar.org/8430/c0b9afa478ae660398704b11dca1221ccf22.pdf)

### 分類
reservoir state ${\bf x}(t)$は入力${\bf u}(t)$を高次元非線形にした信号だと考えられる．
カーネルトリックで低次元だと分類できない物も分類できるようになると考えられる

### 次元削減
fMRIは高次元データなので，どれくらい低次元のデータで状態を表現できるかに興味があったのかな？
PCAかけてた

![](https://i.imgur.com/r3IExPM.jpg)

### 他の手法との比較
- 元の信号での分類
- ブロックごとにくっつける
- ARモデル

## Results
1. 分類の精度が他の手法に比べてよかった
2. トラジェクトリ
    第3主成分まででトラジェクトりをかくと，タスクによって違った
![](https://i.imgur.com/iyvo4ay.jpg)

## Discussion
ReservoirはfMRIデータに使えるということを示した．
他の脳データにも使えるのでは．
"reservoir framework developed here might be particularly useful in characterizing temporal processing of naturalistic stimuli"
"An important working hypothesis in cell datra is that low dimentional neural trajectories provide compact descriptions of underlying processes."
