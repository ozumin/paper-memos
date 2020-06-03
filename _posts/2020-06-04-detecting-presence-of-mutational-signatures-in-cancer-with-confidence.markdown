---
layout: post
title:  "Detecting presence of mutational signatures in cancer with confidence (Huang et al., 2017)"
date:   2020-06-04 00:26:00 +0900
categories: paper
---
[論文](https://pubmed.ncbi.nlm.nih.gov/29028923/)
# TL;DR
遺伝子情報から変異シグネチャーを信頼区間付きで推定したという話．
一つの行列を推定する際にブートストラップで信頼区間つけている．

# 手法
M-PEが目的関数．
Mは変異プロファイルが列ごとに並んだ行列．
Pは事前に求められた変異シグネチャーの行列．
Eが求めるべき行列で， 各変異シグネチャーが生成される確率を表す行列．

Eを推定するのにquadratic programming (QP)とsimulated annealing (SA)を用いた．

Eのsignature contributionがどれくらい信頼度があって安定しているかをブートストラップサンプルを用いて測る．
1000回ブートストラップサンプルして(列のサンプルかな？)，Eを推定して信頼区間を出している．

# 結果
患者ごとの変異プロファイルの生成確率を信頼区間付きで出していた．

# 結論
変異シグネチャーには推定するに当たって安定したものと不安定なものがある．
