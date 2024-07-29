<!--
author: TERAI Yoshihiko
-->
## [ABC259H Yet Another Path Counting](https://atcoder.jp/contests/abc259/tasks/abc259_h)

<details><summary>ヒント1</summary>

もし「同じ値は $10$ 個以下しか現れない」という制約があれば、どのように解けるでしょうか？
</details>

<details><summary>ヒント2</summary>

もし $1\leq a_{i, j}\leq 10$ という制約があれば、どのように解けるでしょうか？
</details>

<details><summary>ヒント3</summary>

マスは $N^2$ 個しかないので、$N$ 回以上現れる値は $N$ 個以下です。
</details>

<details><summary>解説</summary>

もし同じ値が少ない回数しか登場しないのなら、次のアルゴリズム 1. を行えばよいです。

1. 各値について、その値のラベルをもつマスから始点と終点を全探索する。

たとえばヒント 1 の制約では、各ラベルについて高々 $100$ 回のループでこの計算ができることになります。

また、もし登場する値の種類数が少ないなら、次のアルゴリズム 2. を行えばよいです。

2. 値を固定し、その値のラベルを持つマスについて $\mathrm{dp}$ の値を $1$ とする。左上から右下へ経路を計算する DP を走らせ、最後に現在注目しているラベルのマスの $\mathrm{dp}$ の値の総和を取る。

たとえばヒント 2 の制約では、$10$ 回だけ $\mathrm{O}(N^2)$ の DP を行えばよいことになります。

ここで、$N$ 回以上現れる値は $N$ 個以下であることに注目します。$N$ 回以上現れる値にはアルゴリズム 2. を、それ以外にはアルゴリズム 1. をそれぞれ行うと、実は全体の計算量は $\mathrm{O}(N^3)$ になっています。これの証明は本問の公式解説を参照してください。

このように何らかの大小で対象を 2 つ（またはそれ以上）に分け、それぞれで別の処理を行うことで計算量を落とすテクニックは頻出です。
今回のように平方根で対象を分ける（マスの個数 $N^2$ の平方根 $N$ で分けている）テクニックは特に典型で、<b>平方分割</b>という名前がついています。
</details>

<details><summary>実装例 (C++)</summary>

```cpp=
#include <atcoder/modint>
#include <iostream>
#include <utility>
#include <vector>

using namespace std;
using mint = atcoder::modint998244353;

int main() {
    int n;
    cin >> n;
    vector a(n, vector<int>(n));
    for (int i = 0; i < n; i++) {
        for (auto &e : a[i]) {
            cin >> e;
            e--;
        }
    }

    vector<mint> fact(n * n + 1), ifact(n * n + 1);
    fact[0] = 1;
    for (int i = 1; i <= n * n; i++) {
        fact[i] = fact[i - 1] * i;
    }
    ifact[n * n] = fact[n * n].inv();
    for (int i = n * n; i >= 1; i--) {
        ifact[i - 1] = ifact[i] * i;
    }

    auto binom = [&fact, &ifact](int N, int K) -> mint {
        if (N < 0 || K < 0 || N < K) return 0;
        return fact[N] * ifact[K] * ifact[N - K];
    };

    vector<vector<pair<int, int>>> p(n * n);
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            p[a[i][j]].emplace_back(i, j);
        }
    }

    mint ans = 0;

    for (auto &v : p) {
        if ((int)v.size() >= n) { // dp
            vector dp(n, vector<mint>(n));
            for (auto [i, j] : v) dp[i][j] = 1;
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < n; j++) {
                    if (i + 1 < n) dp[i + 1][j] += dp[i][j];
                    if (j + 1 < n) dp[i][j + 1] += dp[i][j];
                }
            }
            for (auto [i, j] : v) ans += dp[i][j];
        } else { // binom
            for (auto [i1, j1] : v) {
                for (auto [i2, j2] : v) {
                    ans += binom(i2 - i1 + j2 - j1, i2 - i1);
                }
            }
        }
    }

    cout << ans.val() << endl;
}
```
</details>
    