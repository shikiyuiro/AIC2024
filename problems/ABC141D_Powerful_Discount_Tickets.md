<!--
author: TERAI Yoshihiko
-->
## [ABC141D Powerful Discount Tickets](https://atcoder.jp/contests/abc141/tasks/abc141_d)

Bonus: $1\leq M \leq 10^9$ の制約で解いてみましょう。

<details><summary>ヒント1</summary>

割引券が $1$ 枚の場合、どの品物に割引券を使うのが最適でしょうか？
</details>

<details><summary>ヒント2</summary>

$\displaystyle \left\lfloor \frac{\left\lfloor\frac{X}{a}\right\rfloor}{b} \right\rfloor = \left\lfloor \frac{X}{ab} \right\rfloor$ が成り立ちます。

つまり、割引券をいっぺんに使うのでなく、1 枚ずつ使うと考えても答えは変わりません。
すると、ヒント 1 での考察が使えます。
</details>

<details><summary>解説</summary>

割引券を $1$ 枚のみ使う場合、最も値段の大きい品物に使うのが最適です。
したがって、次のようなアルゴリズムで答えを得ることができます。

- 最も大きい値段の品物に割引券を使い値段を半分にする、ということを $M$ 回繰り返す。

この操作は Priority Queue と呼ばれる構造を用いると高速に実現できます。Python では <code>heapq</code>、C++ では <code>std::priority_queue</code> を用いると良いです。時間計算量は $\mathrm{O}((N+M)\log N)$ です。
</details>

<details><summary>実装例 (C++)</summary>

```cpp=
#include <iostream>
#include <queue>

using namespace std;

int main() {
    int n, m;
    cin >> n >> m;
    priority_queue<int> que;
    for (int i = 0; i < n; i++) {
        int a;
        cin >> a;
        que.emplace(a);
    }
    for (int i = 0; i < m; i++) {
        int b = que.top();
        que.pop();
        que.emplace(b / 2);
    }
    long long ans = 0;
    for (int i = 0; i < n; i++) {
        ans += que.top();
        que.pop();
    }
    cout << ans << "\n";
}
```
</details>

<details><summary>実装例 (Python)</summary>

```python=
import heapq as pq

n, m = map(int, input().split())
a = list(map(int, input().split()))
for i in range(n):
    a[i] *= -1

pq.heapify(a)
for _ in range(m):
    x = -pq.heappop(a)
    pq.heappush(a, -(x // 2))

ans = 0
for i in a:
    ans += -i

print(ans)
```
</details>

<details><summary>Bonusの解説</summary>

実は元問題と全く同じ解き方をしても大丈夫です。
品物 $i$ に割引券を使う回数は $\mathrm{O}(\log A_i)$ 回なので、計算量は $\mathrm{O}(\min(M, N\log(\max A))\log N)$ となります。
</details>
