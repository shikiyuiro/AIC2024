<!--
author: TERAI Yoshihiko
-->
## [ABC215D Coprime 2](https://atcoder.jp/contests/abc215/tasks/abc215_d)

<details><summary>ヒント1</summary>

条件を満たさない $k$ をすべて求めることにします。つまり、ある $i$ が存在して $\mathrm{gcd}(A_i, k) \neq 1$ であるものをすべて求めることにします。
</details>

<details><summary>ヒント2</summary>

具体的な $\mathrm{gcd}$ の値を求める必要はなく、互いに素かそうでないかだけが重要です。
$\mathrm{gcd}$ が $1$ でないとき、特にその $2$ 数は共通する<b>素因数</b>を持ちます。
</details>

<details><summary>ヒント3</summary>

「$A$ の中に $p$ の倍数が存在する」という素数 $p$ に対し、$p$ の倍数 $k$ は条件を満たしません。逆にそれ以外の数は条件を満たします。
</details>

<details><summary>解説</summary>

「$A$ の中に $p$ の倍数が存在する」というような $M$ 以下の素数 $p$ をすべて求めます。これは $A_i$ をすべて素因数分解することで求めることができます。

その後求めた各素数 $p$ について、$1$ 以上 $M$ 以下の数のうち $p$ の倍数をすべてふるい落とします。最後まで残っている値が条件を満たす数となっています。

この解法は $\mathrm{O}(N\sqrt{\max A} + M\log \log M)$ などで動作します。（エラトステネスの篩と同じループであることから第二項が従います）

最初に素数を列挙することで、同じ素数について $2$ 回以上ふるい落とすことがないことが本質的に計算量を改善していることに注意してください。
</details>

<details><summary>実装例 (C++)</summary>

```cpp=
#include <algorithm>
#include <iostream>
#include <vector>

using namespace std;

vector<int> prime_factor(int x) {
    vector<int> res;
    for (int i = 2; i * i <= x; i++) {
        if (x % i == 0) {
            res.push_back(i);
            while (x % i == 0) x /= i;
        }
    }
    if (x > 1) res.push_back(x);
    return res;
}

int main() {
    int n, m;
    cin >> n >> m;
    vector<int> A_primes;

    for (int i = 0; i < n; i++) {
        int a;
        cin >> a;
        auto d = prime_factor(a);
        for (int x : d) {
            if (x <= m) A_primes.push_back(x);
        }
    }
    sort(A_primes.begin(), A_primes.end());
    A_primes.erase(unique(A_primes.begin(), A_primes.end()), A_primes.end());

    vector<int> ok(m + 1, 1);
    for (int p : A_primes) {
        for (int j = p; j <= m; j += p) ok[j] = 0;
    }

    vector<int> ans;
    for (int i = 1; i <= m; i++)
        if (ok[i]) ans.push_back(i);
    cout << ans.size() << "\n";
    for (int i : ans) cout << i << "\n";
}
```
</details>

<details><summary>実装例 (Python)</summary>

```python=
def prime_factor(x):
    res = []
    i = 2
    while i * i <= x:
        if x % i == 0:
            res.append(i)
            while x % i == 0:
                x //= i
        i += 1
    if x > 1:
        res.append(x)
    return res

n, m = map(int, input().split())
a = list(map(int, input().split()))

A_primes = set()
for x in a:
    for p in prime_factor(x):
        A_primes.add(p)

A_primes = list(A_primes)

ok = [1] * (m + 1)
for p in A_primes:
    for j in range(p, m + 1, p):
        ok[j] = 0

ans = []
for i in range(1, m + 1):
    if ok[i]:
        ans.append(i)

print(len(ans))
for i in ans:
    print(i)
```
</details>

<details><summary>さらに高速な解法</summary>

「$A$ の中に $p$ の倍数が存在する」というような $M$ 以下の素数 $p$ をより高速に列挙する方法を与えます。

$A$ の頻度配列を $B$ とします。$M$ 以下の各素数 $p$ について、$B_p, B_{2p}, B_{3p}, \ldots$ のうち $1$ つでも $0$ でない要素があった場合、$p$ の倍数が $A$ に存在することになります。
この確認はエラトステネスの篩と同じ要領で行うことができ、計算量は $\mathrm{O}(M\log \log M)$ となります。
もとの解法と合わせると、この解法の全体の時間計算量は $\mathrm{O}(N+M\log\log M)$ です。

なお、この問題には線形時間の解法も存在します。詳しくはこの問題のユーザー解説を参照してください。
</details>

<details><summary>補足</summary>

「〇〇という条件を満たすものを数え上げよ」という問題は、「〇〇という条件を<b>満たさない</b>ものを数え上げよ」という問題を解いて、すべての場合の数から引いて求めると筋が良いことがあります。
どちらのほうが数え上げ易いかは当然問題に依るので考察が必要(なことが多い)ですが、今回のように「$\mathrm{gcd}$ を固定して互いに素でないものの場合の数を数える」という問題は非常によく見る典型です。
</details>
