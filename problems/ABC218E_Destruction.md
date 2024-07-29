<!--
author: TERAI Yoshihiko
-->
## [ABC218E Destruction](https://atcoder.jp/contests/abc218/tasks/abc218_e)

<details><summary>ヒント1</summary>

一般に、辺の削除と連結性判定をサポートするデータ構造は難しいです。しかし、辺の追加と連結性判定は効率的に行えます（UnionFind）。
</details>

<details><summary>ヒント2</summary>

$C_i \lt 0$ の辺は明らかに削除する必要がないです。

最初にすべての報酬をもらっておき、$C_i \lt 0$ であるような辺のみが存在する状態から、グラフが連結になるまで辺を追加するようにしましょう。
最初にもらった報酬から、戻した辺の分だけ引かれます。

このとき、追加する辺の $C_i$ の総和はなるべく小さいほうがよいです。
</details>

<details><summary>ヒント3</summary>

$C_i \lt 0$ であるような辺が存在しない場合、ヒント 2 にあるような問題は<b>最小全域木問題</b>と呼ばれる有名問題です。
</details>

<details><summary>解説</summary>

ヒント 3 にあるように、この問題は最小全域木問題を含みます。最小全域木問題に対するアルゴリズムをうまく改造しましょう。

最小全域木を求めるアルゴリズムには例えば以下のものがあります。

- Kruskal 法
- Prim 法
- Borůvka 法

今回の問題は Kruskal 法と相性が良いです。$C_i \lt 0$ である辺のみがグラフにある状態で Kruskal 法を動かせば答えを得られます。

クラスカル法の詳しい説明については補足を参照してください。
</details>

<details><summary>実装例 (C++)</summary>

```cpp=
#include <iostream>
#include <algorithm>
#include <array>
#include <vector>
#include <atcoder/dsu>
using namespace std;

int main() {
    int n, m;
    cin >> n >> m;
    vector<array<int, 3>> edges(m);
    for (int i = 0; i < m; i++) {
        cin >> edges[i][0] >> edges[i][1] >> edges[i][2];
    }
    sort(edges.begin(), edges.end(), [](auto x, auto y){
        return x[2] < y[2];
    });

    atcoder::dsu uf(n);
    long long ans = 0;
    for (auto [a, b, c]: edges) {
        a--, b--;
        if (c < 0) {
            uf.merge(a, b);
        }
        else {
            if (uf.same(a, b)) ans += c;
            else uf.merge(a, b);
        }
    }
    cout << ans << "\n";
}
```
</details>

<details><summary>実装例 (Python)</summary>

```python=
class UnionFind:
    def __init__(self, n: int) -> None:
        self.par = [-1] * n
    
    def find(self, x: int) -> int:
        if self.par[x] < 0:
            return x
        self.par[x] = self.find(self.par[x])
        return self.par[x]

    def unite(self, a: int, b: int) -> bool:
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return False
        if self.par[a] > self.par[b]:
            a, b = b, a
        self.par[a] += self.par[b]
        self.par[b] = a
        return True
    
    def same(self, a: int, b: int):
        return self.find(a) == self.find(b)

n, m = map(int, input().split())
edges = [tuple(map(int, input().split())) for _ in range(m)]
edges.sort(key = lambda x: x[2])

uf = UnionFind(n)
ans = 0
for a, b, c in edges:
    a -= 1
    b -= 1
    if c < 0:
        uf.unite(a, b)
    else:
        f = uf.unite(a, b)
        if not f:
            ans += c

print(ans)
```
</details>

<details><summary>補足</summary>

Kruskal 法は以下のようなアルゴリズムです。

1. グラフを $N$ 頂点 $0$ 辺で初期化する。
2. 辺をコストの昇順にソートする。
3. 各辺について、その端点同士が同じ連結成分に属していない場合、グラフにその辺を追加する。
4. すべての辺を見終わった後、グラフにある辺が最小全域木を成している。

手順 3. は Union-Find を用いると高速にできます。全体の計算量は辺の数を $M$ として $\mathrm{O}(M\log M)$ です。

[Minimum Spanning Tree (Library Checker)](https://judge.yosupo.jp/problem/minimum_spanning_tree)
</details>
