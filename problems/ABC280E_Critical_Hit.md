<!--
author: TERAI Yoshihiko
-->
## [ABC280E Critical Hit](https://atcoder.jp/contests/abc280/tasks/abc280_e)

Bonus: $1\leq N\leq 10^{9}$ の制約で解いてみましょう。

<details><summary>mod 998244353 について</summary>

$\bmod 998244353$ で場合の数/確率/期待値を求めさせる問題が競技プログラミングでは頻出です。これについて慣れていない方は以下の記事を読むとよいです。
https://qiita.com/drken/items/3b4fdf0a78e7a138cd9a

場合の数を $\bmod 998244353$ で求めさせるのは、答えが非常に大きくなってしまう場合があるためです。一方、確率や期待値を $\bmod 998244353$ で出力させるのは、小数で出力させるより都合が良いことが多い（誤差を気にする必要がないなど）ためのことが多いです。

今回の問題でも、実数だと思って様々な考察や式変形を行っても特に問題ありません。
</details>

<details><summary>ヒント</summary>

$N$ が小さいので、算数で解く必要はありません。

$\mathrm{dp}_i :=$ モンスターの体力が $i$ である状態からの攻撃回数の期待値、として $\mathrm{dp}_N$ を求めればよいです。
</details>

<details><summary>解説</summary>

簡単のため、$1$ ダメージ与える確率を $p_1$、$2$ ダメージ与える確率を $p_2$ と表記します。

ヒントにあるような定義で $\mathrm{dp}_N$ を求めます。

- $\mathrm{dp}_0 = 0$
- $\mathrm{dp}_1 = 1$
- $\mathrm{dp}_i = \mathrm{dp}_{i-1}\times p_1 + \mathrm{dp}_{i-2}\times p_2 + 1$ $\ (i\geq 2)$

という式が成り立ちます。これに沿って計算すると、$\mathrm{O}(N)$ で $\mathrm{dp}_N$ を求められます。
</details>

<details><summary>実装例 (C++)</summary>

```cpp=
#include <atcoder/modint>
#include <iostream>
#include <vector>

using namespace std;
using mint = atcoder::modint998244353;

int main() {
    int n, p;
    cin >> n >> p;
    mint p2 = mint(p) / 100, p1 = 1 - p2;

    vector<mint> dp(n + 1);
    dp[1] = 1;
    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i - 1] * p1 + dp[i - 2] * p2 + 1;
    }
    cout << dp[n].val() << "\n";
}
```
</details>

<details><summary>実装例 (Python)</summary>

```python=
mod = 998244353

n, p = map(int, input().split())
p2 = (p * pow(100, -1, mod)) % mod
p1 = (1 - p2) % mod

dp = [0] * (n + 1)
dp[1] = 1
for i in range(2, n + 1):
    dp[i] = (dp[i - 1] * p1 + dp[i - 2] * p2 + 1) % mod

print(dp[n])
```
</details>

<details><summary>Bonusの解説</summary>

$\mathrm{dp}$ の遷移を行列で表すと

\begin{equation}
\left(
\begin{matrix}
\mathrm{dp}_{i} \\
\mathrm{dp}_{i-1} \\
1
\end{matrix}
\right)
= \left(
\begin{matrix} 
p1 & p2 & 1 \\ 
1 & 0 & 0 \\
0 & 0 & 1
\end{matrix}
\right)
\left(
\begin{matrix}
\mathrm{dp}_{i-1} \\
\mathrm{dp}_{i-2} \\
1
\end{matrix}
\right)
\end{equation}

となります。特に、遷移を表す $3\times 3$ 行列が $i$ に依りません。したがって、この行列の累乗を用いて $\mathrm{dp}_N$ を

\begin{equation}
\left(
\begin{matrix}
\mathrm{dp}_{N} \\
\mathrm{dp}_{N-1} \\
1
\end{matrix}
\right)
= \left(
\begin{matrix} 
p1 & p2 & 1 \\ 
1 & 0 & 0 \\
0 & 0 & 1
\end{matrix}
\right)^{N-1}
\left(
\begin{matrix}
\mathrm{dp}_{1} \\
\mathrm{dp}_{0} \\
1
\end{matrix}
\right)
\end{equation}

と表せます。行列の累乗は繰り返し2乗法により $\mathrm{O}(\log N)$ 時間で求められるので、元問題を $\mathrm{O}(\log N)$ 時間で解くことができます。
</details>
    