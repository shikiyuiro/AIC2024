<!--
author: HARADA Kento
-->
## [ABC286G Unique Walk](https://atcoder.jp/contests/abc286/tasks/abc286_g)

<details><summary> ヒント1 </summary>

与えられるグラフに、「ちょうど1回通る辺」と「何回でも通ってよい辺」の2種類があると考えづらいです。グラフを加工することで、辺の種類を1種類に出来ないでしょうか？

</details>

<details><summary> ヒント2 </summary>

「何回でも通ってよい辺」で結ばれた点は、UnionFindを用いて縮約して良いです。これで「ちょうど1回通る辺」のみ考えれば良くなります。

</details>

<details><summary> ヒント3 </summary>

グラフが一筆書きできる条件を知っていますか？

</details>

<details><summary> 解説 </summary>

ヒント2で作ったグラフを考えます。結局、グラフが一筆書きできるか判定できれば良いです。

結論から述べると、全ての頂点の次数が偶数または次数が奇数である頂点がちょうど2つの場合、またその場合に限り、一筆書きが可能です。証明は[こちら](https://manabitimes.jp/math/642)を参照してください。

この条件が成り立つかは $\mathrm{O}(N+M)$ で判定することができるので、この問題を解くことが出来ました。

ちなみに、辺を一本追加するごとに次数の総和は2ずつ増えるので、次数の総和は必ず偶数になります。したがって、次数が奇数である頂点がちょうど1つになることはありません。故に、次数が奇数である頂点が2つ以下であるか判定すれば良いです。

[実装例(C++)](https://atcoder.jp/contests/abc286/submissions/54742538)  
[実装例(Python)](https://atcoder.jp/contests/abc286/submissions/54742589)

</details>
