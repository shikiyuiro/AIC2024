<!--
author: SASAKI Yuma
-->
## [ABC132F Small Products](https://atcoder.jp/contests/abc132/tasks/abc132_f)
    
<details><summary>ヒント1</summary>

素朴な $\mathrm{DP}$ として，
\begin{align*}
\mathrm{dp}[i][j] = (& \text{左から $i$ 個の数を既に決めていて，} \\
&\text{$i$ 番目の数が $j$ であるような場合の数} )
\end{align*}
というものがあります．これの遷移を考えてみましょう．
</details>
    
<details><summary>ヒント2</summary>

$\lfloor \sqrt{N} \rfloor$ より真に大きい数は連続して並ぶことはない，ということを用いて，ヒント１の $\mathrm{DP}$ の状態数を減らしましょう．
</details>
    
<details><summary>ヒント3</summary>

ヒント１の $\mathrm{DP}$ において， $\mathrm{dp}[i][j]$ を $(1\leq i \leq K,1\leq j \leq \lfloor \sqrt{N} \rfloor)$ の範囲で考えましょう．$\lfloor \sqrt{N} \rfloor$ 以下の数をおくときとそれより大きい数をおくときの遷移を別々に考えましょう．
</details>
    
<details><summary> 解説 </summary>
    
[URL](https://drive.google.com/file/d/10DV4_t2EKgP3vHEWme9gUhyCXy1E_G5y/view)
    
<iframe src="https://drive.google.com/file/d/10DV4_t2EKgP3vHEWme9gUhyCXy1E_G5y/preview" width="800" height="500"　allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
    
</details>
    
---
