---
layout: post
title:  "Orchestrated ensemble activities constitute a hippocampal memory engram (Ghandour et al., 2019)"
date:   2020-05-27 09:07:13 +0900
categories: paper
---
[論文](https://www.nature.com/articles/s41467-019-10683-2)

# TL;DR

# 背景
Engram cellという学習に関わるニューロングループがある．
REM睡眠中にmemory replayという現象が起こり，学習していると思われている．
この論文では，engram cellを識別した上で，マウスの海馬CA1のニューロン活動をGCaMP7によるイメージングで見た．
サンプリングレートは20Hz．

# 結果
最初にengram cellがcontextual memoryをどう保持するかを確かめた．
あるON Doxというコンテクスト(環境？)にマウスを置いて，その1日後に同じコンテクストで電気ショックを与える．そうするとON Doxの時に電気ショックを与えた時のようなbehaviorがでる(?)

中略．

#### The total activity of engram is stable across sessions
学習セッションAと寝ている時(B, C, D)と，学習時の環境にした時(E)と異なる環境にした時(F)についてengramとnon-engramの活動を測った．

![experiment](/assets/2020-05-27-4.png)

ニューロン集団の活動がどのように変わるかを見るためにpopulation vector distance (PVD)をengramとnon-engramについて測った．

#### Sub-ensembles construct a single engram population
NMFを使ってニューロングループ(pattern)とその活動に分解した．
基底数はAICで決めた．
Engramとnon-engramは分けて行い，セッションAからFまでも分けて行った．
別々のNMFの基底同士がどれくらい似ているかはcosine類似度で測った．
Engram cellの結果を下に示す．

![S4A](/assets/2020-05-27-5.png)

![S5A](/assets/2020-05-27-6.png)

類似度が0.6以上のグループを同じグループとみなし，そのうち異なるセッションで活動していたもの(aligned)があった．
Non-engram cellでは繰り返し活動するようなグループはかなり少なかった．

さらにmatching score (MS)も導入してセッション同士を比べている．
その結果同じ環境に戻した時(E)の方が異なる環境にした時(F)よりも最初のセッションAに近かった．
これはengram cellだけで見られた．

#### Reactivated ensembles in sleep are reactivated in retrieval
AのあとにEまで残っていたパターンはaligned pattern，一回しか出てこなかったものはisolated patternと呼ぶことにする．
類似度が0.6以上の物を同じパターンとして扱っている．
Aligned patternのうち40％はセッションAのengram cellで構成されている．
Isolated patternのうち80%はnon-engram cellで，セッションに関係がない．

# 手法
NMFはeuclideanを用いた．
1000回初期値を変えて行い，損失が一番小さくなったものを用いた．
AICを用いて基底数を決めた．
時間とニューロン方向にシャッフルした10個のデータをデータとセッションごとに作り．．．
<font color="green">その後どうしたかはどこに書いてあるんだ．
このNMFの結果を照らし合わせたのか？</font>

Matching scoreは異なるセッションのpattern同士についてcosine類似度をはかり，0.6以上だったら1，それ以外は0として作ってる．
