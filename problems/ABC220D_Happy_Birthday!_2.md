<!--
author: HARADA Kento
-->
## [ABC200D Happy Birthday! 2](https://atcoder.jp/contests/abc200/tasks/abc200_d)

<details><summary> ヒント1 </summary>

$N \leq 8$ の場合、どのように解けるでしょうか？

</details>

<details><summary> ヒント2 </summary>

ヒント1の制約下では、$A$ の部分列を全探索することでこの問題を解くことができます。

実は、これと似た解法で $N \leq 200$ の場合も解くことができます。

</details>

<details><summary> ヒント3 </summary>

鳩ノ巣原理を知っていますか？

</details>

<details><summary> 解説 </summary>

この問題では鳩ノ巣原理を使います。これは、「$n+1$ 個のものを $n$ グループに分けると、必ず $2$ つ以上のものが属するグループが存在する」という原理です。

これを使うと、今回の問題では数列を $201$ 個調べれば必ず答えが見つかることが分かります。なぜなら、数列の総和を $200$ で割った余りは $0 \sim 199$ の $200$ 通りしか存在しないためです。

故に、$M = \min(N,8)$ として $A$ の中で前 $M$ 個を取り出して、その中で数列の候補をbit全探索すればよいです。$N < 8$ なら通常のbit全探索と同じです。$n \geq 8$ なら $2^8-1 = 255$ 通りの数列を調べられるので答えを少なくとも一つ見つけることができます。

[実装例(C++)](https://atcoder.jp/contests/abc200/submissions/53997470)  
[実装例(Python)](https://atcoder.jp/contests/abc200/submissions/53997523)

</details>

<details><summary> 類題 </summary>

[類題1 ABC 300点](https://atcoder.jp/contests/abc174/tasks/abc174_c)  
[類題2 ABC 500点](https://atcoder.jp/contests/abc179/tasks/abc179_e)  
[類題3 ARC 500点](https://atcoder.jp/contests/arc170/tasks/arc170_b)

</details>
    
