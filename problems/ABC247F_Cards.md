<!--
author: HARADA Kento
-->
## [ABC247F Cards](https://atcoder.jp/contests/abc247/tasks/abc247_f)

<details><summary> ヒント1 </summary>

「グラフで考える」という典型テクニックを使います。

</details>

<details><summary> ヒント2 </summary>

$N$ 頂点のグラフを考え、各 $i$ について $P_i$ と $Q_i$ の間に無向辺を張りましょう。

</details>

<details><summary> ヒント3 </summary>

ヒント2で作ったグラフにはどのような特徴があるでしょうか。

</details>

<details><summary> 解説 </summary>

$N$ 頂点のグラフを考え、各 $i$ について $P_i$ と $Q_i$ の間に無向辺を張ります。すると、「カードを選ぶ」という行動は「辺を選ぶ」という行動に言い換えることができます。このように言い換えると、問題文で与えられた条件は次のようになります。  
条件：選んだ辺の両端点をすべて集めた集合に、すべての頂点が含まれる（辺被覆と言います）。

また、各頂点の次数は2なので、このグラフはサイクルの集まりになります。したがって、サイクルの場合の問題が解ければ良いです。

一つのサイクルの場合はDPで解くことが出来ます。円環上のDPをする際、「円環を切り開き、端の状態を固定して列の問題に帰着する」という典型テクニックがあります。今回は最初の辺を選ぶか選ばないかで2回DPをすれば、一列に並んでいる場合の問題に帰着することができます。

[実装例 (C++)](https://atcoder.jp/contests/abc247/submissions/54294369)  

</details>

<details> <summary>補足1</summary>

少し実験すると、サイクルのサイズが $1,2,3,4,5$ の場合の答えは $1,3,4,7,11$ となることが分かります。この数列を [OEIS](https://oeis.org/?language=japanese) に入れると、答えがリュカ数になっていることが分かります。時には実験して答えを予想することも大切です。

</details>

<details> <summary>補足2</summary>

今回の問題のように、数列からグラフを作る問題は多いので、どのようなグラフができるか把握しておくと考察がスムーズに進みます。  
長さ $N$ の数列 $A\,(1 \leq A_i \leq N)$ が与えられて、各 $i$ について $i$ から $A_i$ に有向辺を張ると、Functional Graphと呼ばれるグラフになります。  
また、長さ $N$ の順列 $P$ が与えられて、各 $i$ について $i$ から $P_i$ に辺を張ったグラフはサイクルの集まりになります。

</details>
    