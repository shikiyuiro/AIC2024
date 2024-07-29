<!--
author: ISHIKAWA Yuichiro
-->
## [ARC117C Tricolor Pyramid](https://atcoder.jp/contests/arc117/tasks/arc117_c)

<details><summary> ヒント1 </summary>

青を$0$、白を$1$、赤を$2$と置き換えると、置き換わった整数どうしの関係はどのように表されるでしょうか?$\;\mathrm{mod} 3$に注意してみましょう。

</details>

<details><summary> ヒント2 </summary>

実は二項係数を用いて答えが表されます。  
二項係数$\;\mathrm{mod} 3\;$は、Lucasの定理 を用いると高速に求まります。
</details>

<details><summary> 解説 </summary>

あるブロックについて、その下の$2$つのブロックが数$a, b$に対応しているとき、そのブロックは$-(a + b)\;\mathrm{mod} 3\;$に対応していることを示すことができます。したがってパスカルの三角形などの類推から、頂上のブロックは、最下段にあるブロックに対応する数を$d_1, d_2, \ldots, d_N$とするとき、数  
$(-1)^{N-1} \left(\binom{N-1}{0} d_1 + \binom{N-1}{1} d_2 + \ldots + \binom{N-1}{N-1} d_N \right)\;\mathrm{mod} 3\;$  
に対応していることが、数学的帰納法を用いて示されます。  
二項係数$\;\mathrm{mod} 3\;$は、Lucasの定理を用いて高速に計算できる([参考](https://manabitimes.jp/math/1324))ため、合わせて答えがもとまりました。

</details>
