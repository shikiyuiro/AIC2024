<!--
author: SASAKI Yuma
-->
## [ARC023D GCD区間](https://atcoder.jp/contests/arc023/tasks/arc023_4)

<details><summary>ヒント1</summary>

区間の左端を固定して考えます．
</details>
    
<details><summary>ヒント2</summary>
    
$\mathrm{gcd}(a_l , \ldots , a_r)$ を $\mathrm{gcd}(a[l,r])$ と書くことにすると，
\begin{align*}
\mathrm{gcd}(a[l,r]) \geq \mathrm{gcd}(a[l,r+1]) \geq \mathrm{gcd}(a[l,r+2]) \geq \cdots    
\end{align*}
が成り立ちます．
</details>
    
<details><summary>ヒント3</summary>

$\mathrm{gcd}(a[l,r+1])$ は $\mathrm{gcd}(a[l,r])$ の約数になります
</details>
    
<details><summary>ヒント4</summary>

もし， $\mathrm{gcd}(a[l,r]) > \mathrm{gcd}(a[l,r+1])$ ならば， $\mathrm{gcd}(a[l,r+1])$ は $\mathrm{gcd}(a[l,r])$ の半分以下の値になります．
</details>
    
<details><summary> 解説 </summary>
    
[URL](https://drive.google.com/file/d/13_xnmjaAnorWT_2dJv2Oqzh5H4b5mt-x/view)
    
<iframe src="https://drive.google.com/file/d/13_xnmjaAnorWT_2dJv2Oqzh5H4b5mt-x/preview" width="800" height="500"　allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

</details>
