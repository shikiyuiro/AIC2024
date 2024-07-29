<!--
author: TERAI Yoshihiko
-->
## [AGC030D Inversion Sum](https://atcoder.jp/contests/agc030/tasks/agc030_d)


<details><summary>ヒント1</summary>

以前の操作がその後の操作に関わる（操作が独立でない）ので、何らかの形で以前の結果がわかるようになっていなければなりません。
</details>

<details><summary>ヒント2</summary>

DP で解きます。この問題は DP をどう定義するかが全てです。
</details>


<details><summary>ヒント3 (本質的)</summary>

$\mathrm{dp}_{t, i, j} :=$ $t$ 個目の操作まで考え、このとき $A_i \gt A_j$ となっているような場合の数、という DP をします。
</details>

<details><summary>解説</summary>

ヒント3で定義した DP が高速に回ればよいです。特に、各操作について $\mathrm{O}(N)$ 程度で DP の更新ができればこの問題を解くことができます。

実は $\mathrm{dp}_t$ と $\mathrm{dp}_{t+1}$ で値が異なる場所は $\mathrm{O}(N)$ 箇所しかありません。操作に関わるのは $X_i$ と $Y_i$ であるため、$(X_i, k)$ や $(Y_i, k)$ という対についてしか $\mathrm{dp}$ の値が変わらないためです。

実装時は $(i, j)$ と $(j, i)$ を区別し、更新箇所を最初に洗い出して最後に一気に更新するとバグが発生しにくいです。
</details>

<details><summary>類題</summary>

[6問時代 ABC F](https://atcoder.jp/contests/abc176/tasks/abc176_f)
[yukicoder No.1515](https://yukicoder.me/problems/no/1515)
</details>