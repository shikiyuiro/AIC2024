<!--
author: TERAI Yoshihiko
-->
## [ABC333D Erase Leaves](https://atcoder.jp/contests/abc333/tasks/abc333_d)

<details><summary> ヒント1 </summary>

頂点 1 が削除されるということは、その直前で頂点 1 は葉であり、すなわち次数が 1 であるということです。
</details>

<details><summary> ヒント2 </summary>

頂点 1 の次数を減らすにはどのような操作をすればよいでしょう？また、どのようにすれば最適な操作となるでしょう？
</details>


<details><summary> 解説 </summary>

頂点 1 の次数を $d$ とします。

頂点 $1$ を削除したグラフを考えると、グラフは $d$ 個の木に分かれます。最後の操作の直前では頂点 1 の次数は 1 であるため、それ以前にこれら $d$ 個の連結成分のうち $d-1$ 個は消されていなければなりません。

消す $d-1$ 個の連結成分はサイズの小さいものから選んでいくのがもちろん最善なので、小さいほうから $d-1$ 個の連結成分のサイズの和を求めれば答えが求まります。これは木 dp で求める事ができます。

実際は、$n$ から最も大きい連結成分のサイズを引くことで答えを求めるのが簡潔です。
</details>

<details>
    <summary>実装例 (C++)</summary>

- 再帰による実装（ラムダ再帰）
```cpp=
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int n;
    cin >> n;
    vector<vector<int>> tree(n);
    for (int i = 0; i < n - 1; i++) {
        int u, v;
        cin >> u >> v;
        u--, v--;
        tree[u].push_back(v);
        tree[v].push_back(u);
    }

    vector<int> sz(n);
    auto dfs = [&](auto&& self, int v, int parent) -> int {
        sz[v] = 1;
        for (int c: tree[v]) {
            if (c == parent) continue;
            sz[v] += self(self, c, v);
        }
        return sz[v];
    };

    dfs(dfs, 0, -1);

    int mx = 0;
    for (int i = 1; i < n; i++) mx = max(mx, sz[i]);
    cout << n - mx << endl;
}
```

- 非再帰の実装
```cpp=
#include <algorithm>
#include <iostream>
#include <queue>
#include <vector>
using namespace std;

int main() {
    int n;
    cin >> n;
    vector<vector<int>> tree(n);
    for (int i = 0; i < n - 1; i++) {
        int u, v;
        cin >> u >> v;
        u--, v--;
        tree[u].push_back(v);
        tree[v].push_back(u);
    }

    vector<int> bfs_order;

    // BFS
    vector<int> dist(n, n);
    dist[0] = 0;
    queue<int> que;
    que.push(0);
    while (!que.empty()) {
        int v = que.front();
        que.pop();
        bfs_order.push_back(v);
        for (int c : tree[v]) {
            if (dist[c] == n) {
                dist[c] = dist[v] + 1;
                que.push(c);
            }
        }
    }

    reverse(bfs_order.begin(), bfs_order.end());

    vector<int> sz(n);
    for (int v : bfs_order) {
        sz[v] = 1;
        for (int c : tree[v]) {
            if (dist[c] < dist[v]) continue;
            sz[v] += sz[c];
        }
    }

    int mx = 0;
    for (int i = 1; i < n; i++) mx = max(mx, sz[i]);
    cout << n - mx << endl;
}
```
</details>

<details>
    <summary>実装例 (Python)</summary>

```python=
from collections import deque

n = int(input())
tree = [[] for _ in range(n)]
for _ in range(n - 1):
    u, v = map(int, input().split())
    u, v = u - 1, v - 1
    tree[u].append(v)
    tree[v].append(u)

# BFS
bfs_order = []
que = deque()
dist = [n] * n
que.append(0)
dist[0] = 0

while len(que) > 0:
    v = que.popleft()
    bfs_order.append(v)
    for c in tree[v]:
        if dist[c] == n:
            dist[c] = dist[v] + 1
            que.append(c)

bfs_order = bfs_order[::-1]

sz = [1] * n

for v in bfs_order:
    for c in tree[v]:
        if dist[c] < dist[v]:
            continue
        sz[v] += sz[c]

mx = max(sz[1:])
print(n - mx)
```
</details>

<details>
    <summary>補足</summary>

木 DP は BFS 順序の逆順にループを回すことで非再帰で計算することができます。海外ジャッジでは木の頂点数が $10^6$ 程度である問題も多く、再帰だと想定解でも TLE する場合があります。ただし実装は再帰のほうが軽いため、制約等で使い分けられると強いです。
（発展的な話題：さらに BFS 順序昇順で更新を行いながら全方位木 DP も行うことができます。）

各言語の実装は非再帰で書いているため、是非参考にしてください。
</details>
