---
layout: post
title:  "Reservoir Computing Properties of Neural Dynamics in Prefrontal Cortex (Enel et al., 2016)"
date:   2020-04-25 01:07:13 +0900
categories: paper
---
[論文](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4902312/)
# TL;DR
タスクかけたサルの脳の振る舞いと同じタスクで学習させたreservoirの振る舞いを比べた．

# 背景・問題
mixed selectivityというキーワードが出てくる．
Prefrontal cortex (PFC)で見られる現象のようで，ランダムなネットワークが入力によって活動する部分が違うこと？

あとは実際の脳とreservoir computingの関係の紹介とかされている．

この論文では一個のニューロンとニューロン集団を両方みる．

# 手法
Reservoirはworking memoryとlanguage comprihension and productionのを使った？
前者は1つのニューロンの解析で，後者はcontext encodingに使った．
Reservoirのパラメタとか表現できるダイナミクスが多い方が良いが，そのパラメタは
"Fixed network parameters include networks size (1000 neurons), and standard values for input sparsity (10%), internal reservoir connection sparsity(10%), and spectral radios (0.9)"となっている．
<font color="Green">実際の皮質ニューロンのコネクティビティが11%くらいなのでこれ面白いなと思った．</font>

データは2匹のサルの546ニューロンで，local field potentialもとっている．
場所はdorsal snterior cingulate cortex (dACC)．

タスクは，4つのボタンをサルに選ばせるもの．
1つ正解があって，それが押されると多分報酬がでる．
しばらくすると正解のボタンが変わる．
サルは最初探索し，正解がわかると繰り返すという，searchとrepeatのフェーズがある．

Reservoirは二種類で，オリジナルとcontextual memoryがあるものが使われた．
2つ目はcontextual encodingの有用性を示すため．
入力はサルがもらえるような5次元（レバー，選んだボタン，報酬など）．
出力はサルでも同じ8次元で，最初の4つは視線と，後の4つは手がどこを触ったか．

Contextual memoryでは，seachかrepetitionかというフェーズの出力も1つ足された．

学習にはFORCEを使い，それにdelayを組み込んでうまく行ったらしい．
Delayはよく読んでないけど，ちょっと前のoutputをフィードバックすることみたい．
# 結果
Reservoirの精度は十分よかったので，中身を見ていた．
Target choiceとphaseに関連するニューロンのfiring rateを取った（それにはANOVAを使った）．
それぞれについて発火頻度が異なるニューロンがあったらしい(Fig3)．

サルのニューロンについても同じことをやると同じ傾向があった(Fig4)．

他にもいろいろ見てた．
例えばPCAかけて第3主成分までを3次元空間にプロットしてトラジェクトリを見たりしてた．
これはphaseありなしで比べてた．

<font color="Green">読むの力尽きてしまった</font>
