<!--
author: SASAKI Yuma
-->
## [ABC162E Sum of gcd of Tuples (Hard)](https://atcoder.jp/contests/abc162/tasks/abc162_e)

<details><summary> ヒント１ </summary>
    
寄与で考えます．すなわち， $\mathrm{gcd}$ が $g(1\leq g \leq K)$ であるものが何通りかを各 $g$ について求めます．
    
</details>

<details><summary> ヒント２ </summary>
    
$\mathrm{gcd}$ が $g$ の倍数である $\Leftrightarrow$ 数列の各要素が $g$ の倍数である
    
</details>

<details><summary> ヒント３ </summary>
    
$\mathrm{gcd}$ が $g$ である $\Leftrightarrow$ $\mathrm{gcd}$ が $g$ の倍数である かつ $\mathrm{gcd}$ が $2g,3g,\ldots,\lfloor \frac{K}{g} \rfloor g$ でない
</details>

<details><summary> 解説</summary>
    
まず初めに， $\mathrm{gcd}$ が $g$ の倍数であるような数列の個数について考えます．
ヒントにもあるように， $\mathrm{gcd}$ が $g$ であるということは，数列の各要素が $g$ の倍数であることと同値なので，これを数えられればよいです．
$K$ 以下の $g$ の倍数の個数は， $\lfloor \frac{K}{g} \rfloor$ 個であり，長さ $N$ の数列なので，求めたい個数は $\displaystyle {\lfloor \frac{K}{g} \rfloor}^N$となります．

次にこれを用いて $\mathrm{gcd}$ が $g$ であるような数列の個数について求めましょう． $\mathrm{gcd}$ が $g$ であるような数列の個数を $\mathrm{num}[g]$ と書くことにすると，ヒントより，
$\displaystyle \mathrm{num}[g] =  {\lfloor \frac{K}{g} \rfloor}^N - \mathrm{num}[2g] - \mathrm{num}[3g] - \cdots - \mathrm{num}[\lfloor \frac{K}{g} \rfloor g]$
と書けることが分かります．式より， $\mathrm{num}[g]$ は $g<g'$ なる全ての $g'$ における $\mathrm{num}[g']$ の値が分かっていれば求められることが分かるので，これを用いて，動的計画法によって $g$ の降順に $\mathrm{num}[g]$ の値が計算できることが分かりました．

最後に，上記解法の計算量を考察します．まず，各 $g$ について計算する　${\lfloor \frac{K}{g} \rfloor}^N$ ですが，これは二分累乗法などで $O(\log N)$ で計算できます．次に，
$- \mathrm{num}[2g] - \mathrm{num}[3g] - \cdots - \mathrm{num}[\lfloor \frac{K}{g} \rfloor g]$
の部分ですが，これは各 $g$ について $\lfloor \frac{K}{g} \rfloor$ 個以下の項であるため，調和級数のオーダー評価により，全体で
$\displaystyle \sum_{g=1}^{K} \lfloor \frac{K}{g} \rfloor = O(K \log K)$
個の項であることが分かります．以上より，上記解法の時間計算量は $O(K( \log K+ \log N))$ と評価できます．実装は素直です．

本解法のように，「余分に数えた分を引く」という方法を除原理などと言ったりします．数え上げにおいて頻出の考え方ですので，ぜひ使えるようになりましょう．
    
</details>
