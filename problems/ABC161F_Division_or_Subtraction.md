<!--
author: TERAI Yoshihiko
-->
## [ABC161F Division or Subtraction](https://atcoder.jp/contests/abc161/tasks/abc161_f)

<details><summary>ヒント1</summary>

簡単のため、$K$ で割る操作を操作1、$K$ を引く操作を操作2と呼ぶことにします。

まずは適当な $N$ と $K$ で実験をしてみましょう。特に、操作列がどのようになっているかに注目しましょう。
</details>

<details><summary>ヒント2</summary>

一度操作1ができなくなったら、操作2をすることで再び操作1が可能になることはありません。
これは操作2によって $N \bmod K$ の値が変化しないことからわかります。
したがって、操作2はまとめて $N \to N \bmod K$ と行えます。
</details>

<details><summary>ヒント3</summary>

ヒント2と同様の議論から、操作列は $1\to 1\to \cdots \to 1\to 2\to 2\to \cdots \to 2$ となっています。
特に、操作1を行うなら必ず最初の操作は操作1であり、$K$ は $N$ の約数となります。
</details>

<details><summary>解説</summary>

$K$ をひとつ固定したとき、$K$ が条件を満たすかは $\mathrm{O}( \log N)$ 時間で確かめられます。操作1は $\mathrm{O}(\log N)$ 回しか行われず、操作2は1回にまとめることができるからです。

条件を満たす $K$ の最初の操作が操作1か操作2かで場合分けします。

最初の操作が操作1のとき、$K$ として考えられるのは $N$ の約数です。$N$ の約数の個数は小さいので（[高度合成数一覧](https://algo-method.com/descriptions/92)などを参照）、これはすべて試すことができます。

最初の操作が操作2であるとき、$N \equiv 1 \pmod K$ です。両辺から $1$ を引くと $N-1\equiv 0 \pmod K$ となり、これは $K$ が $N-1$ の約数であることと同値です。よって $1$ を除く $N-1$ の約数はすべて条件を満たすことになります。

結局 $N$ と $N-1$ の約数がすべてわかればこの問題を解くことができ、全体の計算量は $\mathrm{O}(\sqrt{N} + d(N)\log N)$ です。（$d(N)$ は $N$ の約数の個数）
</details>

<details><summary>実装例 (Python)</summary>

```python=
def divisors(n):
    d = []
    i = 1
    while i * i <= n:
        if n % i == 0:
            d.append(i)
            if i * i < n:
                d.append(n // i)
        i += 1
    return sorted(d)

n = int(input())
d1 = divisors(n)
d2 = divisors(n - 1)

ans = len(d2) - 1

for k in d1:
    if k == 1:
        continue
    N = n
    while N % k == 0:
        N //= k
    if N % k == 1:
        ans += 1

print(ans)
```
</details>

<details><summary>実装例 (C++)</summary>

```cpp=
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;
using i64 = long long;

vector<i64> divisors(i64 n) {
    vector<i64> res;
    for (i64 i = 1; i * i <= n; i++) {
        if (n % i == 0) {
            res.push_back(i);
            if (i * i < n) {
                res.push_back(n / i);
            }
        }
    }
    sort(res.begin(), res.end());
    return res;
}

int main() {
    i64 n;
    cin >> n;

    auto d1 = divisors(n);
    auto d2 = divisors(n - 1);

    int ans = (int)d2.size() - 1;

    for (i64 k : d1) {
        if (k == 1) continue;
        i64 N = n;
        while (N % k == 0) N /= k;
        if (N % k == 1) ans++;
    }

    cout << ans << endl;
}
```
</details>
