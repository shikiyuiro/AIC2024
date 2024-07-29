<!--
author: HARADA Kento
-->
## [ABC079D Wall](https://atcoder.jp/contests/abc079/tasks/abc079_d)

<details><summary> ヒント1 </summary>

操作を何回か繰り返して数字 $i$ を数字 $1$ にするとき、必要な魔力の最小値が求められれば良いです。

</details>

<details><summary> ヒント2 </summary>

この問題は、グラフの問題に言い換えることができます。

</details>

<details><summary> ヒント3 </summary>

$10$ 頂点のグラフを考え、頂点 $i$ から頂点 $j$ に距離 $c_{i,j}$の辺を張りましょう。

</details>

<details><summary> 解説 </summary>

今回の問題のように、コストがかかる操作を何回か繰り返すとき、かかるコストの総和の最小値を求める問題はグラフの最短経路問題に帰着できることがあります。
[類題 ABC400点](https://atcoder.jp/contests/abc224/tasks/abc224_d?lang=ja)

ヒント3で述べたように、$10$ 頂点のグラフを考え、頂点 $i$ から頂点 $j$ に距離 $c_{i,j}$の辺を張ります。すると、$i$ を $1$ に変えるまでにかかる魔力の最小値は、頂点 $i$ から頂点 $1$ への最短経路長と一致します。

グラフの各頂点間の最短経路はワーシャルフロイド法などによって求められるので、数字 $i$ を数字 $1$ にするのに必要な魔力の最小値は $K=10$ として $\mathrm{O}(K^3)$ で求めることが出来ます。（ダイクストラ法を使うとより高速に求めることができますが、今回の問題ではそこまで高速にする必要が無いので、実装が簡単なワーシャルフロイド法をおすすめします。）

後は、各 $A_{i,j}\,(1 \leq i \leq H, 1 \leq j \leq W,A_{i,j} \neq -1)$ について $A_{i,j}$ を $1$ にするのに必要なコストを足し合わせると答えが求められます。

以上より、この問題を解くことができました。計算量は $\mathrm{O}(K^3+HW)$ です。

[実装例 (C++)](https://atcoder.jp/contests/abc079/submissions/54223597)  
[実装例 (Python)](https://atcoder.jp/contests/abc079/submissions/54268430)

</details>
