<!--
author: HARADA Kento
-->
## [ABC196D Hanjo](https://atcoder.jp/contests/abc196/tasks/abc196_d)

<details><summary> ヒント1 </summary>

サンプルを見ると、答えはそこまで大きくならないのでは？という予想が出来ます。

</details>

<details><summary> ヒント2 </summary>

再帰関数を用いた全探索ができます。

</details>

<details><summary> 解説 </summary>

数え上げの問題では、最後のサンプルが最大ケースになっている場合があります。そのような場合は、そのサンプルの出力例の大きさを確認しましょう。もし小さければ、それを利用した解法があるかもしれません。

今回の問題では最大ケースであるかは分かりませんが、答えがある程度大きくなりそうなサンプルがサンプル3に置いてあります。この値が小さいことから、全探索が可能ではないか？と疑いましょう。

上記のような予想を踏まえて、答えの上界を見積もります。タイルを配置し終えた後、各マスは
- 縦に置いた長方形のタイルに含まれる
- 横に置いた長方形のタイルに含まれる
- 正方形のタイルに含まれる  

のいずれかになります。最大 $16$ 個のマスについて、$3$ 通りの状態があるので、答えは $3^{16}$ 通り以下になります。これは十分小さいので、どのような配置方法があるかを全探索することで、この問題を解くことができます。再帰関数を用いた実装がおすすめです。詳しくは実装例をご覧ください。

[実装例(C++)](https://atcoder.jp/contests/abc196/submissions/54709846)  
[実装例(Python)](https://atcoder.jp/contests/abc196/submissions/54709874)

</details>

<details><summary> 補足1 </summary>
実は、2021年のICPC模擬国内予選で似た問題が出題されています。余力のある方は解いてみましょう。

[問題リンク](https://onlinejudge.u-aizu.ac.jp/services/ice/?problemId=3248)

</details>

<details><summary> 補足2 </summary>
ABC-Fにこの問題の強化版が出題されたことがあります。余力のある方は解いてみましょう。

[問題リンク](https://atcoder.jp/contests/abc204/tasks/abc204_f)

</details>
