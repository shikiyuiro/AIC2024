<!--
author: HARADA Kento
-->
## [ABC295E Kth Number](https://atcoder.jp/contests/abc295/tasks/abc295_e)

<details><summary> ヒント </summary>

求める期待値は  
$\sum_{i=1}^{M} (i \times (K番目に大きい数がiになる確率))$ 
と表せます。この式を式変形しましょう。

</details>

<details><summary> 解説 </summary>

$K$ 番目に大きい数が $i$ になる確率を $p_i$ とします。すると、求める期待値は  $\sum_{i=1}^{M} (i \times p_i)$ と表せます。この式を式変形します。  

例えば、$M = 5$ とします。すると、$\sum_{i=1}^{M} (i \times p_i)$  は以下の図で青い部分の面積に対応します。

![before](https://hackmd.io/_uploads/SyplH-v4C.png)


上図について少し視点を変えると、求める期待値は次の図のオレンジの部分の面積と一致します。

![after](https://hackmd.io/_uploads/rJ_WrWvEA.png)


故に、$K$ 番目に大きい数が $i$ 以上になる確率を $q_i$ とすると、  
$\sum_{i=1}^{M} (i \times p_i) = \sum_{i=1}^{M}q_i$  
と変形することができます。

$q_i$ は「操作1を行った後 $A_j \geq i$ を満たす $j$ が $N+1-K$ 個以上ある確率」です。これは、$A$ に含まれる $0$ の内 $i$ 以上に置き換えられるものの数を全探索することで求めることができます。計算量は実装方針によって $\mathrm{O}(NM)$ や $\mathrm{O}(NM \log N)$ になります。

[実装例(C++)](https://atcoder.jp/contests/abc295/submissions/54045481)

</details>

