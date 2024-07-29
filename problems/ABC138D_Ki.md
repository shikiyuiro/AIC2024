<!--
author: TERAI Yoshihiko
-->
## [ABC138D Ki](https://atcoder.jp/contests/abc138/tasks/abc138_d)

<details><summary> ヒント1 </summary>
    
すべてのクエリの処理が終わった後の状態さえわかればよい、というのが重要です。途中の結果を保存する必要はなく、自由にクエリの順序を入れ替えることも許されます。
</details>

<details><summary> ヒント2 </summary>
    
木の問題を考えるときは、まずパスグラフの場合を考える典型があります。パスグラフは基本的に列と同じなので、次のような問題になります：

- 長さ $N$ の列 $A$ があり、最初はすべての要素が $0$ である。「$A_{p_j}, A_{p_j+1}, \ldots, A_N$ に $x_j$ を足す」というクエリを $Q$ 個処理したあとの $A$ の状態を出力せよ。
</details>

<details><summary> ヒント3 </summary>
    
列の場合の問題は「imos 法」と呼ばれるテクニックにより $\mathrm{O}(N+Q)$ 時間で解けます。imos 法を知らない人は、先にこれを調べてから取り組んでみましょう。
</details>

<details><summary> ヒント4 </summary>
    
各頂点のカウンタの値は、「自分の祖先（自分と頂点 $1$ を結ぶパス上の頂点）へのクエリの値の総和」になります。
これを踏まえて、木上の DFS の探索順序に思いを馳せましょう。
</details>


<details><summary> 解説 </summary>

説明を簡単にするため、各頂点に $0$ で初期化されたカウンタ 2 を設置します。クエリの際は、一時的に頂点 $p_j$ のカウンタ 2 に $x_j$ を足しておき、最後にまとめて集計することにします。

各頂点の最終的なカウンタの値は、「自身および自身の祖先のカウンタ 2 の値の総和」になります。このことから、次のようなアルゴリズムで答えを得ることができます。

1. 変数 $x$ を $0$ で初期化し、頂点 $1$ から DFS を開始する。
2. DFS で頂点 $v$ に訪れたとする。$x$ に $v$ のカウンタ 2 の値を加え、$v$ のカウンタの値を $x$ にする。
3. $v$ の子に潜る。

これは本質的に木上で imos 法を行っていることになります。
    
このアルゴリズムでうまくいくことがあまりしっくりこない場合は、サンプルで手を動かして実際に動きを見てみるとよいかもしれません。
</details>
<details><summary>実装例 (C++)</summary>

```cpp=
#include <bits/stdc++.h>
using namespace std;
using i64 = long long;

vector<int> tree[200010];
i64 counter[200010], counter2[200010];

void dfs(int v, int parent, i64 x) {
    x += counter2[v];
    counter[v] = x;
    for (int child : tree[v]) {
        if (child == parent) continue;
        dfs(child, v, x);
    }
}

int main() {
    int n, q;
    cin >> n >> q;
    for (int i = 0; i < n - 1; i++) {
        int a, b;
        cin >> a >> b;
        a--, b--;
        tree[a].push_back(b);
        tree[b].push_back(a);
    }
    for (int j = 0; j < q; j++) {
        int p, x;
        cin >> p >> x;
        counter2[p - 1] += x;
    }
    dfs(0, -1, 0);
    for (int i = 0; i < n; i++) cout << counter[i] << " ";
}
```

</details>
    
<details>
<summary>実装例 (Python)</summary>

```python=
import sys
# 再帰上限の変更
sys.setrecursionlimit(300000)

n, q = map(int, input().split())
tree = [[] for _ in range(n)]
for i in range(n - 1):
    a, b = map(int, input().split())
    a, b = a - 1, b - 1
    tree[a].append(b)
    tree[b].append(a)

counter = [0] * n
counter2 = [0] * n

def dfs(v, parent, x):
    x += counter2[v]
    counter[v] = x
    for child in tree[v]:
        if child == parent:
            continue
        dfs(child, v, x)

for j in range(q):
    p, x = map(int, input().split())
    counter2[p - 1] += x

dfs(0, -1, 0)

print(*counter)
```
</details>
    
<details>
<summary>補足1</summary>

列に対する何らかの操作を木に拡張した問題はとてもよく見ます。例えば、次のような列の問題は見たことがあるかと思います：
- 長さ $N$ の整数列 $A$ が与えられる。「$A_{l}, A_{l+1}, \ldots, A_{r}$ の最小値を求めよ」というクエリに $Q$ 回答えよ。(https://judge.yosupo.jp/problem/staticrmq)

これは Segment Tree や Sparse Table というデータ構造を用いて $\mathrm{\tilde{O}}(N+Q)$ で解くことができます。これを木の場合に拡張すると、次のような問題になります。

1. $N$ 頂点の木が与えられる。頂点 $i$ には値 $A_i$ が定まっている。「頂点 $u$ と頂点 $v$ を結ぶパス上の頂点の値の最小値を求めよ」というクエリに $Q$ 回答えよ。
2. $N$ 頂点の木が与えられる。頂点 $i$ には値 $A_i$ が定まっている。「頂点 $v$ の部分木にある頂点の値の最小値を求めよ」というクエリに $Q$ 回答えよ。

今回の問題は「木上で imos 法をせよ」という問題でした。
    
Bonus: 上に挙げた 1. 2. を $N, Q \leq 2\times 10^5$ 程度の制約で解いてみましょう。（1. は ABC-F 525 点、2. は ABC-E 450 点程度です）
</details>
    
<details>
<summary>補足2（水以上向け）</summary>

Euler Tour という概念があります。
https://maspypy.com/euler-tour-%E3%81%AE%E3%81%8A%E5%8B%89%E5%BC%B7

Euler Tour の本質は、「ある頂点を根とする部分木」が「Euler Tour 上のある区間」に対応することです。したがって、与えられた木に対する Euler Tour を求めれば、列に対する imos 法そのものになります。

また Euler Tour を取れば、今回のクエリをオンラインで処理することもできます。つまり、設定は同じに次の 2 種類のクエリを処理できるようになります。

1. 頂点 $p_j$ の部分木のカウンタに $x_j$ を足す。
2. 頂点 $p_j$ のカウンタの値を出力する。

ぜひ考えてみてください。
</details>
