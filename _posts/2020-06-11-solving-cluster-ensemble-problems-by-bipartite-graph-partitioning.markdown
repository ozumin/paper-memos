---
layout: post
title:  "Solving Cluster Ensemble Problems by Bipartite Graph Partitioning (Fern et al., 2004)"
date:   2020-06-11 01:07:13 +0900
categories: paper
---
[論文](https://dl.acm.org/doi/abs/10.1145/1015330.1015414)
# TL;DR
Cluster ensembleでは最後のクラスタリング結果を得るのが困難．

# 背景・問題
クラスタリングのアンサンブルをgraph partitionの問題として考えるのが2002年から出てきた．

この論文の例ではハードクラスタリングしているが，ソフトクラスタリングとしても使える．

# Existing graph formulations for cluster ensembles
## Instance-based graph formulation (IBGF)
データ同士を同じクラスタに属しているか否かで類似度行列を作成し，それを既存のgraph partitioning技術でクラスタに分ける．
計算コストがかかる．

## Cluster-based graph formulation (CBGF)
クラスタをノード，エッジをクラスタ間のJaccard係数としてgraph partitionする．
ここでの仮定は，クラスタはさらに上位のmeta clusterに所属するというもので，そのmeta clusterを見つけにいってる（？）

# Hybrid bipartite graph formulation (HBGF, 提案手法)
ノードはクラスタとデータで，エッジはデータがクラスタに入っていたら1，入っていなかったら0．
二部グラフになる．
<font color='green'>このあとgraph partitionするんだよねきっと</font>

# 実験
データを70％ランダムにサブサンプルしたのと，適当な行列で射影して縮約したデータをK-meansでクラスタリングした．
Kは事前にデータセットによって決めた．
Graph partitionにはspectral graph partitioningとmultilevel graph partitionを用いた．

結果はデータセットによって異なった．
