<!--
author: TERAI Yoshihiko
-->
## [ABC202E Count Descendants](https://atcoder.jp/contests/abc202/tasks/abc202_e)

<details><summary>ヒント1</summary>

クエリを言い換えると次のようになります：

- 頂点 $U_i$ を根とする部分木であって、深さが $D_i$ であるものの個数を求めよ。
</details>
    
<details><summary>ヒント2</summary>

部分木に対するクエリと言われたら、まずは Euler Tour （DFS の行きがけ順に頂点を並べたもの）を考るのが典型です。
Euler Tour を取ると、部分木の情報は Euler Tour における区間になることを思い出しましょう。
</details>

<details><summary>ヒント3</summary>

与えられた木の Euler Tour を $T$ とおくと、各クエリは次のようになります：

- 整数 $u_l, u_r$ が与えられるので、$T_{u_l}, T_{u_l+1}, \ldots, T_{u_r}$ のうち深さが $D_i$ であるものの個数を求めよ。

ただし $u_l, u_r$ は頂点 $U_i$ を根とする部分木に対応する Euler Tour の区間の端点です。
</details>

<details><summary>解説</summary>

ヒント 3 までの議論から、Euler Tour の各頂点を各頂点の深さに代えた配列 $A$ に対する次のクエリが高速に捌ければよいです。

- 整数 $L, R, X$ が与えられる。$A_L, A_{L+1}, \ldots, A_R$ のうち、$X$ に等しいものの個数を求めよ。

実はこの問題と全く同じ問題が ABC で既出です。[ABC248D](https://atcoder.jp/contests/abc248/tasks/abc248_d)

これは [Static Range Frequency](https://judge.yosupo.jp/problem/static_range_frequency) と呼ばれる有名問題です。
この問題の解き方はこの問題の解説に任せることにします。

これにより全体で $\mathrm{O}(N)$ や $\mathrm{O}(N\log N)$ 時間でこの問題を解くことができます。
</details>

<details><summary>より一般的なテクニック</summary>

今回の問題は、各頂点 $v$ の部分木の深さの頻度配列が分かれば解けるのでした。これは木 DP で連想配列を管理し、集計時に Weighted Union Heuristic （いわゆるマージテク）を行うことでも実現できます。
C++ における <code>std::map</code> だと $\mathrm{O}(N(\log N)^2)$、C++ における <code>std::unordered_map</code> や Python における <code>defaultdict</code> を用いると $\mathrm{O}(N\log N)$ $\mathrm{expected}$ となります。定数倍が軽くないため、書き方によっては落ちるかもしれません。その代わり、実装量は比較的軽いです。

また、部分木に対するクエリを捌く強力なテクニックに <b>DSU on Tree</b> と呼ばれるものがあります。
[参考資料](https://speakerdeck.com/camypaper/dsu-on-tree)

DSU on Tree を用いると、部分木の頻度配列を定数倍軽めの $\mathrm{O}(N\log N)$ で求められます。

C++ にはそれぞれの実装例をつけたので、是非参考にしてみてください。

なおここに挙げたテクニックはオフラインクエリである場合のみ可能であることに注意してください。
</details>

<details><summary>実装例 (C++)</summary>

- Euler Tour + Static Range Frequency $\mathrm{O}(N\log N)$
```cpp=
#include <iostream>
#include <vector>

using namespace std;

int main() {
    int n;
    cin >> n;
    vector<vector<int>> tree(n);
    for (int i = 1; i < n; i++) {
        int p;
        cin >> p;
        tree[p - 1].push_back(i);
    }

    vector<int> A(n);
    vector<int> in(n), out(n);

    int t = 0;
    auto euler_tour = [&](auto&& self, int v,int d) -> void {
        A[t] = d;
        in[v] = t;
        t++;
        for (int c : tree[v]) {
            self(self, c, d + 1);
        }
        out[v] = t;
    };
    euler_tour(euler_tour, 0, 0);

    vector<vector<int>> B(n + 1);
    for (int i = 0; i < n; i++) B[A[i]].push_back(i);

    auto range_frequency = [&](int l, int r, int x) -> int {
        return lower_bound(B[x].begin(), B[x].end(), r) -
               lower_bound(B[x].begin(), B[x].end(), l);
    };

    int q;
    cin >> q;
    while (q--) {
        int u, d;
        cin >> u >> d;
        u--;
        cout << range_frequency(in[u], out[u], d) << "\n";
    }
}
```

- <code>std::unordered_map</code> + Weighted Union Heuristic $\mathrm{O}(N\log N)$ $\mathrm{expected}$

```cpp=
#include <algorithm>
#include <iostream>
#include <queue>
#include <unordered_map>
#include <vector>

int main() {
    int n;
    std::cin >> n;
    std::vector<std::vector<int>> tree(n);
    for (int i = 1; i < n; i++) {
        int p;
        std::cin >> p;
        tree[p - 1].push_back(i);
    }
    int q;
    std::cin >> q;
    std::vector<std::vector<std::pair<int, int>>> query(n);
    for (int i = 0; i < q; i++) {
        int u, d;
        std::cin >> u >> d;
        query[u - 1].emplace_back(d, i);
    }
    std::vector<int> ans(q);

    std::vector<int> bfs_ord;
    std::vector<int> depth(n, n);
    std::queue<int> que;
    que.emplace(0);
    depth[0] = 0;
    while (!que.empty()) {
        int v = que.front();
        que.pop();
        bfs_ord.push_back(v);
        for (int c: tree[v]) {
            if (depth[c] == n) {
                depth[c] = depth[v] + 1;
                que.emplace(c);
            }
        }
    }

    std::reverse(bfs_ord.begin(), bfs_ord.end());

    std::vector<std::unordered_map<int, int>> dp(n);
    for (int v: bfs_ord) {
        for (int c: tree[v]) {
            if (dp[v].size() < dp[c].size()) {
                std::swap(dp[v], dp[c]);
            }
            for (auto [i, j]: dp[c]) dp[v][i] += j;
        }
        dp[v][depth[v]]++;
        for (auto [d, i]: query[v]) ans[i] = dp[v][d];
    }
    
    for (int i = 0; i < q; i++) std::cout << ans[i] << "\n";
}
```

- DSU on Tree $\mathrm{O}(N\log N)$
```cpp=
#include <algorithm>
#include <functional>
#include <iostream>
#include <vector>

int main() {
    int n;
    std::cin >> n;
    std::vector<std::vector<int>> tree(n);
    for (int i = 1; i < n; i++) {
        int p;
        std::cin >> p;
        tree[p - 1].push_back(i);
    }
    int q;
    std::cin >> q;
    std::vector<std::vector<std::pair<int, int>>> query(n);
    for (int i = 0; i < q; i++) {
        int u, d;
        std::cin >> u >> d;
        query[u - 1].emplace_back(d, i);
    }
    std::vector<int> ans(q);
    std::vector<int> depth(n), sz(n);

    auto predfs = [&](auto&& self, int v, int t) -> int {
        depth[v] = t;
        for (int c : tree[v]) {
            sz[v] += self(self, c, t + 1);
        }
        for (int i = 1; i < (int)tree[v].size(); i++) {
            if (sz[tree[v][0]] < sz[tree[v][i]]) {
                std::swap(tree[v][0], tree[v][i]);
            }
        }
        sz[v]++;
        return sz[v];
    };
    predfs(predfs, 0, 0);

    std::vector<int> table(n);

    auto add_subtree = [&](auto&& self, int v) -> void {
        table[depth[v]]++;
        for (int c : tree[v]) self(self, c);
    };

    auto clear_subtree = [&](auto&& self, int v) -> void {
        table[depth[v]]--;
        for (int c : tree[v]) self(self, c);
    };

    auto dfs = [&](auto&& self, int v, bool f) -> void {
        for (int i = 1; i < (int)tree[v].size(); i++) {
            self(self, tree[v][i], true);
        }
        if (!tree[v].empty()) {
            self(self, tree[v][0], false);
        }
        for (int i = 1; i < (int)tree[v].size(); i++) {
            add_subtree(add_subtree, tree[v][i]);
        }
        table[depth[v]]++;
        for (auto [d, i] : query[v]) {
            ans[i] = table[d];
        }
        if (f) clear_subtree(clear_subtree, v);
        return;
    };

    dfs(dfs, 0, 0);

    for (int i = 0; i < q; i++) std::cout << ans[i] << "\n";
}
```
</details>

<details><summary>実装例 (Python)</summary>

- Euler Tour + Static Range Frequency $\mathrm{O}(N\log N)$
```python=
import sys
import bisect

sys.setrecursionlimit(10 ** 6)

n = int(input())
p = list(map(int, input().split()))

tree = [[] for _ in range(n)]
for i in range(1, n):
    tree[p[i - 1] - 1].append(i)

A = [-1] * n
t = 0
in_time = [0] * n
out_time = [0] * n

def euler_tour(v, d):
    global t
    A[t] = d
    in_time[v] = t
    t += 1
    for c in tree[v]:
        euler_tour(c, d + 1)
    out_time[v] = t

euler_tour(0, 0)

B = [[] for _ in range(n + 1)]
for i in range(n):
    B[A[i]].append(i)

def range_frequency(l, r, x):
    return bisect.bisect_left(B[x], r) - bisect.bisect_left(B[x], l)

q = int(input())
for _ in range(q):
    U, D = map(int, input().split())
    U -= 1
    print(range_frequency(in_time[U], out_time[U], D))
```


- <code>defaultdict</code> + Weighted Union Heuristic $\mathrm{O}(N\log N)$ $\mathrm{expected}$

```python=
from collections import deque, defaultdict

n = int(input())
p = list(map(int, input().split()))

tree = [[] for _ in range(n)]
for i in range(1, n):
    tree[p[i - 1] - 1].append(i)

bfs_order = []
depth = [n] * n
depth[0] = 0
que = deque()
que.append(0)
while que:
    v = que.popleft()
    bfs_order.append(v)
    for c in tree[v]:
        depth[c] = depth[v] + 1
        que.append(c)

q = int(input())
querys = [[] for _ in range(n)]
for i in range(q):
    u, d = map(int, input().split())
    u -= 1
    querys[u].append((i, d))

ans = [0] * q

dp = [defaultdict(int) for _ in range(n)]

for v in reversed(bfs_order):
    dp[v][depth[v]] = 1
    for c in tree[v]:
        if len(dp[v]) < len(dp[c]):
            dp[v], dp[c] = dp[c], dp[v]
        for x, y in dp[c].items():
            dp[v][x] += y
    
    for i, d in querys[v]:
        ans[i] = dp[v][d]

print(*ans, sep = "\n")
```
</details>

