<!--
author: TERAI Yoshihiko
-->
## [ABC199D RGB Coloring 2](https://atcoder.jp/contests/abc199/tasks/abc199_d)

<details><summary>ヒント1</summary>

明らかに連結成分ごとに独立なので、連結成分ごとに問題を解き、それらの積を取ればよいです。
特に、グラフが連結である場合に高速に解ければよいです。（ほとんど自明な帰着ですが、非常によく見る典型的な単純化です。）
</details>

<details><summary>ヒント2</summary>

赤く塗る頂点を固定したとき、全体を条件を満たすように塗り分けられるか？という問題を考えましょう（数え上げでなく判定問題であることに注意）。どのようなグラフであれば条件を満たすでしょうか？

赤く塗らないと決めた頂点は残りの $2$ 色でしか塗れないのがポイントです。
</details>

<details><summary>ヒント3</summary>

赤く塗る頂点の決め打ち方は $2^N$ 個しかありません。
</details>

<details><summary>解説</summary>

赤く塗る頂点を決め打ったとき、残りの頂点を条件を満たすように塗り分けられる必要十分条件は、赤く塗ると決めた頂点（およびそれに接続する辺）を削除したグラフが二部グラフであることです。この判定は $\mathrm{O}(M)$ 時間で行えます。
またグラフが二部グラフであるとき、ある連結成分を $2$ 色で塗り分ける場合の数は明らかに $2$ です。よって、二部グラフ判定ができれば場合の数も求まることになります。

全体の時間計算量は $\mathrm{O}((N+M)2^N)$ などになります。
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
g = [[] for _ in range(n)]
for _ in range(m):
    a, b = map(int, input().split())
    a -= 1
    b -= 1
    g[a].append(b)
    g[b].append(a)

def is_bipartite(b: int):
    uf = UnionFind(n * 2)
    for i in range(n):
        if b >> i & 1:
            continue
        for j in g[i]:
            if b >> j & 1:
                continue
            uf.unite(i, j + n)
            uf.unite(j, i + n)
            if uf.same(i, i + n):
                return False
    return True

ans = 0

for b in range(1 << n):
    f = True
    for i in range(n):
        for j in g[i]:
            if (b >> i & 1) and (b >> j & 1):
                f = False
                break
    if not f:
        continue
    if not is_bipartite(b):
        continue
    c = n - b.bit_count()
    uf = UnionFind(n)
    for i in range(n):
        if b >> i & 1:
            continue
        for j in g[i]:
            if b >> j & 1:
                continue
            c -= uf.unite(i, j)
    ans += 1 << c

print(ans)
```
</details>

<details><summary>補足</summary>

二部グラフ判定は BFS または DFS による $\mathrm{O}(N+M)$ 時間の判定、UnionFind を用いた $\mathrm{O}(M\alpha (N))$ 時間の判定が知られています。
DFS/BFS は隣接頂点と異なる色で頂点を塗っていけばよいだけです。UnionFind による二部グラフ判定を紹介している記事を以下に貼ります。

https://noshi91.hatenablog.com/entry/2018/04/17/183132

またこの手法を改善することで辺追加/二部グラフ判定クエリをオンラインで捌くこともできます。余力のある人は考えてみましょう。
</details>
