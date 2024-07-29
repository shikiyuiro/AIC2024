<!--
author: TERAI Yoshihiko
-->
## [ABC170D Not Divisible](https://atcoder.jp/contests/abc170/tasks/abc170_d)

<details><summary>ヒント1</summary>


$A$ の中に $A_i$ の約数が存在しないような $i$ の個数を求めよ、という問題です。

$M:= \max A$ とします。

素直に $A_i$ の約数をすべて調べる方針では $\mathrm{\Theta}(M + N\sqrt{M})$ の計算量になります。
（最近の言語アップデートでこの方針でも通ってしまうのですが）より高速な方法を考えてみましょう。
</details>

<details><summary>ヒント2</summary>

約数に比べ、倍数のほうがはるかに扱いやすいことが多いです。
$M$ の値が小さいという制約の弱点にも注目しましょう。
</details>

<details><summary>ヒント3</summary>

$A_i$ を固定し、$A$ に存在する $A_i$ の倍数をすべてふるい落とす、という操作の後に $A$ に残っている要素の個数が答えです。
</details>

<details><summary>解説</summary>

$\mathrm{cnt}[x] :=$ （$A_i = x$ となる $i$ の個数）という配列（頻度配列）を用意し、$A$ に含まれる各 $A_i$ について $2A_i, 3A_i, \ldots$ の $\mathrm{cnt}$ の値を $0$ にしていきます。最後に $\mathrm{cnt}$ の総和を取れば答えになります。
このアルゴリズムの計算量は、調和級数の部分和の議論から
$$\sum_{i=1}^M \left\lfloor \frac{M}{i} \right\rfloor \leq
M \sum_{i=1}^M \left\lfloor \frac{1}{i} \right\rfloor \leq
M(\log M + 1) = \mathrm{O}(M\log M)$$
より $\mathrm{O}(N + M\log M)$ となります。

$A$ の中に同じ値が複数存在する場合の処理に注意してください。（例えばすべての $i$ で $A_i = 1$ のとき、同じ値をスキップしないと $\Theta(NM)$ の計算量がかかります）
</details>

<details><summary>実装例 (Python)</summary>

```python=
M = 10 ** 6
n = int(input())
a = list(map(int, input().split()))

cnt = [0] * (M + 1)
for i in range(n):
    cnt[a[i]] += 1

ans = 0
for x in a:
    if cnt[x] == 0:
        continue
    for y in range(x + x, M + 1, x):
        cnt[y] = 0
    if cnt[x] > 1:
        cnt[x] = 0

print(sum(cnt))
```
</details>

<details><summary>実装例 (C++)</summary>

```cpp=
#include <iostream>
#include <vector>
using namespace std;

const int M = 1'000'000;

int main() {
    int n;
    cin >> n;
    vector<int> a(n);
    for (int i = 0; i < n; i++) cin >> a[i];

    vector<int> cnt(M + 1);
    for (int i = 0; i < n; i++) cnt[a[i]]++;

    for (int x : a) {
        if (cnt[x] == 0) continue;
        for (int y = x + x; y <= M; y += x) {
            cnt[y] = 0;
        }
        if (cnt[x] > 1) cnt[x] = 0;
    }

    int ans = 0;
    for (int i = 1; i <= M; i++) ans += cnt[i];
    cout << ans << endl;
}
```
</details>

<details><summary>さらに高速な解法</summary>

$B$ を $A$ の頻度配列とし、$C_i := \sum_{j\mid i}B_j$ で非負整数列 $C$ を定義します。このとき、$B_i = C_i = 1$ となる $i$ の個数が本問題の答えとなります。（少し難しいです）

$C$ は高速ゼータ変換を用いて $\mathrm{O}(M\log \log M)$ で求められるので、全体で $\mathrm{O}(N + M\log \log M)$ 時間でこの問題を解くことができます。

（自然な実装をしたところ、定数倍の関係で $\mathrm{O}(N+M\log M)$ 時間解法のほうが AtCoder のジャッジでは高速でした）
</details>

<details><summary>実装例2 (Python)</summary>

```python=
M = 10 ** 6
n = int(input())
a = list(map(int, input().split()))

primes = []
table = [1] * (M + 1)
for i in range(2,  M + 1):
    if table[i] == 1:
        primes.append(i)
    for j in primes:
        if i * j > M:
            break
        table[i * j] = 0

B = [0] * (M + 1)
for x in a:
    B[x] += 1

C = B[:]
for p in primes:
    for j in range(1, M + 1):
        if j * p > M:
            break
        C[j * p] += C[j]

ans = 0
for i in range(M + 1):
    if B[i] == C[i] == 1:
        ans += 1

print(ans)
```
</details>
    
<details><summary>実装例2 (C++)</summary>

```cpp=
#include <iostream>
#include <vector>
using namespace std;

const int M = 1'000'000;

int main() {
    // linear sieve
    vector<int> primes, table(M + 1, 1);
    for (int i = 2; i <= M; i++) {
        if (table[i] == 1) {
            primes.push_back(i);
        }
        for (int j : primes) {
            if (i * j > M) break;
            table[i * j] = 0;
        }
    }

    int n;
    cin >> n;
    vector<int> B(M + 1);
    for (int i = 0; i < n; i++) {
        int a;
        cin >> a;
        B[a]++;
    }
    vector<int> C = B;
    for (int p : primes) {
        for (int j = 1; j * p <= M; j++) {
            C[j * p] += C[j];
        }
    }

    int ans = 0;
    for (int i = 1; i <= M; i++) {
        if (B[i] == 1 && C[i] == 1) ans++;
    }

    cout << ans << endl;
}
```
</details>
