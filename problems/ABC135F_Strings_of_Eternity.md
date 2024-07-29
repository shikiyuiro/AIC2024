<!--
author: TERAI Yoshihiko
-->
## [ABC135F Strings of Eternity](https://atcoder.jp/contests/abc135/tasks/abc135_f)

<details>
    <summary>ヒント用の記法</summary>

- $n:=s$ の長さ
- $m:=t$ の長さ
- $s_i:=$ $s$ の $i$ 番目の文字
- $s^{\infty} :=$ $s$ を無限個連結した文字列
</details>
    
<details>
    <summary>ヒント1</summary>

問題文を言い換えると次のようになります：
- $s^{\infty}$ の中に $t$ は最大何回連続して現れるか求めよ。
</details>
    
<details>
    <summary>ヒント2</summary>

状態を頂点、操作を辺とみなしたグラフを考えることで見通しが良くなる場合があります。これは非常によく見る典型です。

本問題の答えは、$s^{\infty}$ の各文字を頂点、$s^{\infty}_i$ からの $m$ 文字が $t$ に等しいときに $s^{\infty}_i \to s^{\infty}_{i+m}$ と辺を張ったグラフの最長経路長に等しいです。

しかしこのグラフは無限個の頂点を持つので、陽に構築することはできません。
</details>
    
<details>
    <summary>ヒント3</summary>

$s^{\infty}$ の長さは無限なので、$s^{\infty}$ の $i$ 文字目以降の文字列は $s^{\infty}$ の $i+n$ 文字目以降の文字列と等しいです。これを用いて、ヒント $2$ で構築したグラフの頂点数を有限にできます。
</details>

<details>
    <summary>ヒント4</summary>

$s$ の各文字を頂点、$s^{\infty}_i$ からの $m$ 文字が $t$ に等しいときに $s_i \to s_{(i+m) \bmod n}$ と辺を張ったグラフを考えましょう。
</details>
    
<details>
    <summary>解説</summary>

ヒント4に書いたようなグラフ $G$ を考えると、$G$ 上の最長経路長が本問題の答えになります。これは以下のように求められます。
- $G$ に有向サイクルが含まれる場合、答えは $\infty$ である。
- $G$ に有向サイクルが含まれない場合、$G$ は DAG となっているので、DP によって答えを求められる。

あとはヒント4にあるグラフを構築する方法を与えればよいです。つまり、$s^{\infty}_i$ からの $m$ 文字が $t$ に等しいか否かを $0\leq i\lt n$ について高速に列挙できればよいです。

様々な方法がありますが、$t + s^{\infty}$ という文字列に対して Z-Algorithm を行うのが最も簡単かと思われます。Z-Algorithm は AtCoder Library にも実装されているので、そちらのドキュメントを読むとよいです。
または KMP 法や Rolling Hash というアルゴリズムでも実現することが可能です。Rolling Hash は特に汎用性の高いアルゴリズムなので、持っておいても良いかもしれません。C++ の実装例には Rolling Hash を用いています。
</details>
    
<details>
    <summary>実装例 (C++)</summary>
    
- Z-algorithm
```cpp=
#include <bits/stdc++.h>
using namespace std;
#include <atcoder/string>

int main() {
    string s, t;
    cin >> s >> t;

    string s_infty;
    while (s_infty.size() < t.size()) s_infty += s;
    s_infty += s_infty;

    int n = s.size(), m = t.size();

    string z_tmp = t + '$' + s_infty;
    vector<int> Z = atcoder::z_algorithm(z_tmp);

    vector<int> g(n, -1), deg(n);
    for (int i = 0; i < n; i++) {
        if (Z[i + (m + 1)] == m) {
            g[i] = (i + m) % n;
            deg[(i + m) % n] = 1;
        }
    }

    // topological sort -> cycle detection
    vector<int> top;
    queue<int> que;
    for (int i = 0; i < n; i++)
        if (deg[i] == 0) que.push(i);

    while (!que.empty()) {
        int v = que.front();
        que.pop();
        top.push_back(v);
        if (g[v] >= 0) {
            deg[g[v]] = 0;
            que.push(g[v]);
        }
    }

    if (*max_element(deg.begin(), deg.end()) > 0) {
        cout << -1 << "\n";
        exit(0);
    }

    vector<int> dp(n, -1e9);
    reverse(top.begin(), top.end());
    for (int v : top) {
        if (g[v] >= 0) {
            dp[v] = max(0, dp[g[v]]) + 1;
        }
    }

    cout << max(0, *max_element(dp.begin(), dp.end())) << "\n";
}
```

- Rolling Hash
```cpp=
#include <bits/stdc++.h>
using namespace std;

#include <algorithm>
#include <string>
#include <vector>

using u64 = unsigned long long;

struct RollingHash {
    std::vector<u64> table, basepow;
    static u64 base;
    static const u64 MOD = (1ull << 61) - 1;
    static const u64 mask30 = (1ull << 30) - 1;
    static const u64 P = MOD * 4;
    static const u64 mask31 = (1ull << 31) - 1;

    static u64 CalcMod(u64 x) {
        u64 res = (x >> 61) + (x & MOD);
        if (res >= MOD) res -= MOD;
        return res;
    }

    static u64 Mul(u64 a, u64 b) {
        u64 au = a >> 31;
        u64 ad = a & mask31;
        u64 bu = b >> 31;
        u64 bd = b & mask31;
        u64 mid = ad * bu + au * bd;
        u64 midu = mid >> 30;
        u64 midd = mid & mask30;
        return au * bu * 2 + midu + (midd << 31) + ad * bd;
    }

    RollingHash() : RollingHash("") {}
    RollingHash(std::string s) {
        int n = s.size();
        table = std::vector<u64>(n + 1);
        basepow = std::vector<u64>(n + 1, 1);
        for (int i = 0; i < n; i++) {
            basepow[i + 1] = CalcMod(Mul(basepow[i], base));
            table[i + 1] = CalcMod(Mul(table[i], base) + s[i]);
        }
    }

    u64 slice(int l, int r) {
        return CalcMod(table[r] + P - Mul(table[l], basepow[r - l]));
    }

    int lcp(int l, int r) {
        int hi = table.size() - std::max(l, r), lw = 0;
        while (hi - lw > 1) {
            int mid = (hi + lw) / 2;
            if (slice(l, l + mid) == slice(r, r + mid))
                lw = mid;
            else
                hi = mid;
        }
        return lw;
    }
};

u64 RollingHash::base = 10007;

int main() {
    string s, t;
    cin >> s >> t;

    string s_infty;
    while (s_infty.size() < t.size()) s_infty += s;
    s_infty += s_infty;

    int n = s.size(), m = t.size();

    RollingHash S(s_infty), T(t);

    vector<int> g(n, -1), deg(n);
    for (int i = 0; i < n; i++) {
        if (S.slice(i, i + m) == T.slice(0, m)) {
            g[i] = (i + m) % n;
            deg[(i + m) % n] = 1;
        }
    }

    // topological sort -> cycle detection
    vector<int> top;
    queue<int> que;
    for (int i = 0; i < n; i++)
        if (deg[i] == 0) que.push(i);

    while (!que.empty()) {
        int v = que.front();
        que.pop();
        top.push_back(v);
        if (g[v] >= 0) {
            deg[g[v]] = 0;
            que.push(g[v]);
        }
    }

    if (*max_element(deg.begin(), deg.end()) > 0) {
        cout << -1 << "\n";
        exit(0);
    }

    vector<int> dp(n, -1e9);
    reverse(top.begin(), top.end());
    for (int v : top) {
        if (g[v] >= 0) {
            dp[v] = max(0, dp[g[v]]) + 1;
        }
    }

    cout << max(0, *max_element(dp.begin(), dp.end())) << "\n";
}
```
</details>
    
<details>
    <summary>実装例 (Python)</summary>
    
```python=
from collections import deque

def z_algorithm(v):
    n = len(v)
    res = [0] * n
    res[0] = n
    i, j = 1, 0
    while i < n:
        while(i + j < n and v[j] == v[i + j]):
            j += 1
        res[i] = j
        if j == 0:
            i += 1
            continue
        k = 1
        while(k < j and k + res[k] < j):
            res[i + k] = res[k]
            k += 1
        
        i += k
        j -= k
    
    return res

s = input()
t = input()

n = len(s)
m = len(t)

s_infty = s * ((n + m - 1) // n)
s_infty += s_infty

z_tmp = t + '$' + s_infty
Z = z_algorithm(z_tmp)

g = [-1] * n
deg = [0] * n

for i in range(n):
    if (Z[i + (m + 1)] == m):
        g[i] = (i + m) % n
        deg[(i + m) % n] = 1

# topological sort -> cycle detection
top = []
d = deque()
for i in range(n):
    if deg[i] == 0:
        d.append(i)

while len(d) > 0:
    v = d.popleft()
    top.append(v)
    if g[v] >= 0:
        deg[g[v]] = 0
        d.append(g[v])

if max(deg) == 1:
    print(-1)
    exit()

top = top[::-1]

dp = [-10 ** 9] * n

for v in top:
    if g[v] >= 0:
        dp[v] = max(0, dp[g[v]]) + 1

print(max(0, max(dp)))
```
</details>
