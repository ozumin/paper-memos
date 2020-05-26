---
layout: post
title:  "Network Homeostasis and StateDynamics of Neocortical Sleep (Watson et al., 2016)"
date:   2020-05-27 00:07:13 +0900
categories: paper
---
[論文](https://www.sciencedirect.com/science/article/pii/S0896627316300563)
# TL;DR
睡眠時と覚醒時のニューロンの振る舞いの違い

## 結果
11匹のラットの前頭皮質にプローブを挿し，WAKE, nonREM, REMに渡って計測した．
Up stateやDown stateも計測できた．
995個のputative pyramidal cellsと126個のputative interneuronsが計測できた．

Pyramidalの発火率の中央値はWAKE, 0.76 ± 1.53 Hz; nonREM, 0.69 ± 0.86 Hz;REM, 0.88 ± 1.33 Hzで，
interneuronsの発火率の中央値はWAKE, 5.59 ± 7.25 Hz; nonREM, 4.69 ± 5.62 Hz; REM,4.25 ± 9.43 Hz．
<font color="green">internuronsの方がだいぶ発火率大きい．分布も同じような感じなのかな？</font>
Pyramidalの発火率の(累積)分布は以下の通りで裾が長い．

![pyramidal-fire1](/assets/2020-05-27-1.png)
![pyramidal-fire2](/assets/2020-05-27-2.png)

また，nonREMとWAKEでのニューロンごとの発火率は相関がある．

![state-fire](/assets/2020-05-27-3.png)

Pyramidal neuronはバーストする．
この論文ではスパイク間が15ms以下だったらバーストという定義をしていて，その割合がどれくらいかをバーストインデックスとしている．
nonREMでバーストインデックスが最も高かった．

同期現象についても見ていて，50msのoverlapping time windowの中でpopulationとして発火してるかを見た．
その結果15%は滅多に同期せず，4.2%以下は半分以上の窓で同期発火していた．
nonREMは全てのニューロンが静かになる窓で占められており，特徴的だった．
DOWN stateと関係していると思われる．

これ以降もstateごとのfiring rateなどについて論じられている．
