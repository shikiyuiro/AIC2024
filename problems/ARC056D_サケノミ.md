<!--
author: SASAKI Yuma
-->
## [ARC056D サケノミ](https://atcoder.jp/contests/arc056/tasks/arc056_d)
    
<details><summary>ヒント1</summary>
    
時間軸に沿って $dp$ することが目標です．
</details>

<details><summary>ヒント2</summary>

$i$ 番目のグラスに時刻 $t_{i,j}$ に注がれるドリンクを飲むための条件を考えてみましょう．
</details>
    

<details><summary>ヒント3</summary>
    
$i$ 番目のグラスに時刻 $t_{i,j}$ に注がれるドリンクを飲むのは，
    
- $t_{i,j-1} < t < t_{i,j}$ なる時刻 $t$ にドリンクを飲み干した．
- $t_{i,j} < t$ なる時刻 $t$ にドリンクを飲み干した．
                   
の $2$ 条件で表すことができます．
</details>

<details><summary>ヒント4</summary>

$dp_i = \text{最後に時刻 i にドリンクを飲み干したときの最高得点}$ という $dp$ を考えます．このとき，更新式は，
\begin{align*}
dp_j = \max_{i<j}(dp_i + \text{ヒント3の条件を満たす $t_{k,l}$ が存在するような $k$ における $w_k$ の総和})
\end{align*}
となります．ここで寄与分解を考えます．すなわち，各 $t_{k,l}$ について，そいつが理由で $w_k$ が足される $dp_i$ はどのような分布をしているか考えてみましょう．
</details>
    
<details><summary> 解説 </summary>
    
[URL](https://drive.google.com/file/d/13fVPkVYXiLR00DOUQDwG-6tBIpv0nU4J/view)
    
<iframe src="https://drive.google.com/file/d/13fVPkVYXiLR00DOUQDwG-6tBIpv0nU4J/preview" width="800" height="500"　allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
    
</details>
