---
layout: post
title:  "Realistic spiking neuron statistics in a population are described
by a single parametric distribution (Crow, 2018)"
date:   2020-03-21 01:07:13 +0900
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
[論文](https://www.semanticscholar.org/paper/Realistic-Spiking-Neuron-Statistics-in-a-Population-Crow/e6ae310321d2f345f6bd7adc21be279d782715de)

2日に一本という目標だったのにもう破ってしまった...
# TL;DR
スパイクが起きる統計をみてみたよって話

# 背景・問題
ニューロンが発火してから次の発火が起こる時間はランダムのように見えるが，静止膜電位に戻ってから次に発火するまでは別の要因があると考えられる．
そのため，interspike interval (ISI)を調べることで，ニューロンがある時刻での発火のしやすさを知ることができる．
この論文ではMLとAICを用いてスパイク間の時間のパラメトリック分布推定を行う．

# 手法
使う分布は指数分布とガンマ分布とそれぞれの混合分布．

データはサルの視覚野のニューロン約70個．
ニューロンの発火率($$\frac{\text{スパイク数}}{\text{時間}}$$)が以下の図になる．

![発火率(多分1sあたり)](/assets/2020-03-21-1.png)

また，coefficient of variation (CV)という量も計算しており，$$CV = \frac{\text{ISIの標準偏差}}{\text{ISIの平均}}$$，その結果が以下の図になる．

![CV](/assets/2020-03-21-2.png)

# 結果
MLでは，ガンマガンマ混合分布が一番良かった．
AICやBICの結果，指数分布が一番良かったが，ISIが小さいところはうまく捉えられていなかった．
なぜ指数分布が選ばれたのか．
選ばれた分布のロバスト性が次から述べられている．

Spiking neuron modelを作り分布推定したところ，指数分布が一番よくガンマガンマの対数尤度は一番大きかった．
しかし，データと照らし合わせるとガンマガンマの方がよく見える．
これは，AICのパラメタ数のペナルティが大きいのではないかと述べている．

# 結論
この論文では，実データに置いてML以外の指標をみてみて，ISIがどの分布に従ってるかを推定した．
<font color="Green">"reasonable firing rates with a wide range, likely including excitatory neurons (low rates) and inhibitory neurons (higher rates)"が気になった．"A firing rate of 199.75 Hz is not uncommon for fast-spiking inhibitory neurons"とも書いてあった．</font>
