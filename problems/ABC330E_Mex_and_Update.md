<!--
author: HARADA Kento
-->
## [ABC330E Mex and Update](https://atcoder.jp/contests/abc330/tasks/abc330_e)

<details><summary> ヒント1 </summary>

長さ $N$ の数列の $\mathrm{mex}$ は $N$ 以下です。

</details>

<details><summary> ヒント2 </summary>

ヒント1より、$A$ の要素の内 $N$ より大きいものは無視して考えて良いです。

</details>

<details><summary> ヒント3 </summary>

数列 $A$ に含まれない要素を管理しましょう。

</details>

<details><summary> 解説 </summary>

$\mathrm{mex}$ を考える際の典型テクニックとして、「非負整数の中で数列に含まれない要素の集合を管理する」というものがあります。そのような集合を $S$ とすると、答えは $S$ の最小値です。

「非負整数の中で数列に含まれない要素」は無限個存在します。しかし、長さ $N$ の数列の $\mathrm{mex}$ は 必ず $N$ 以下になるので、$N$ より大きい値は無視して良いです。

よって、「非負整数の中で数列に含まれない要素の内、$N$ 以下のもの」のみを管理すれば十分です。これはset(C++)や[sorted_set](https://github.com/tatyam-prime/SortedSet)(Python,tatyamさん作)で実現可能です。

[実装例(C++)](https://atcoder.jp/contests/abc330/submissions/54045934)  
[実装例(Python)](https://atcoder.jp/contests/abc330/submissions/54046059)

</details>

