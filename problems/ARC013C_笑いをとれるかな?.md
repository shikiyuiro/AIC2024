<!--
author: TERAI Yoshihiko
-->
## [ARC013C 笑いを取れるかな？](https://atcoder.jp/contests/arc013/tasks/arc013_3)

<details>
<summary>ヒント1</summary>

典型として、その問題に真に含まれる簡単なバージョンを考えることがあります。今回の場合、次のようなゲームを考えてみましょう。

1. 3 次元でなく、1 次元や 2 次元の場合
2. 豆腐が 1 つの場合
</details>

<details>
<summary>ヒント2</summary>

2 人の操作に関わらず、各豆腐についての「これ以上は操作ができない状態」は一意に定まります。
</details>

<details>
<summary>ヒント3</summary>

すべてのハバネロを含む最小の直方体を考えると、これがその豆腐の終了状態（操作不能な状態）となっています。
</details>

<details>
<summary>ヒント4</summary>

Nim と呼ばれるゲームに帰着されます。Nim について知らない方は調べてみましょう。
</details>
    
<details>
<summary>解説</summary>

まず豆腐が 1 つ、かつ 2 次元の場合を考えてみます。

豆腐の内部に、すべてのハバネロが含まれる最小の長方形を取ります。この長方形は操作によって欠けることがありません。また、この長方形より大きく豆腐が残っていれば、まだこの豆腐に操作をすることが可能です。したがって、もう豆腐に操作が行えない状態とは、この長方形のみが残っている状態ということになります。
    
さて、初期状態でこの長方形の上側に $u$、下側に $d$、左側に $l$、右側に $r$ だけ幅が残っているとします。このとき、プレイヤーは 1 回の操作で「$u, d, l, r$ のうち $1$ 以上であるものを選び、そこから $1$ 以上を引く。ただし引いた後の値が負になってはいけない。」という操作をすることになります。これは $4$ 山の Nim そのものです。

3 次元かつ $N$ 個の豆腐がある場合に戻ります。先程の議論から、このゲームは $6N$ 個の山がある Nim と等価であることがわかりました。あとは Nim の必勝者を求める手順でこのゲームの必勝者を求めることができます。
</details>
    
<details>
    <summary>実装例 (C++)</summary>

```cpp=
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n;
    cin >> n;
    int grundy_number = 0;
    for (int i = 0; i < n; i++) {
        int X, Y, Z;
        cin >> X >> Y >> Z;
        int M;
        cin >> M;
        int xl = X, xr = 0;
        int yl = Y, yr = 0;
        int zl = Z, zr = 0;
        for (int j = 0; j < M; j++) {
            int x, y, z;
            cin >> x >> y >> z;
            xl = min(xl, x);
            xr = max(xr, x + 1);
            yl  = min(yl, y);
            yr = max(yr, y + 1);
            zl = min(zl, z);
            zr = max(zr, z + 1);
        }
        grundy_number ^= xl;
        grundy_number ^= (X - xr);
        grundy_number ^= yl;
        grundy_number ^= (Y - yr);
        grundy_number ^= zl;
        grundy_number ^= (Z - zr);
    }
    if (grundy_number == 0) cout << "LOSE\n";
    else cout << "WIN\n";
}
```
</details>
    
<details>
    <summary>実装例 (Python)</summary>
    
```python=
n = int(input())
grundy_number = 0
for i in range(n):
    X, Y, Z = map(int, input().split())
    M = int(input())
    xl, xr = X, 0
    yl, yr = Y, 0
    zl, zr = Z, 0
    for j in range(M):
        x, y, z = map(int, input().split())
        xl = min(xl, x)
        xr = max(xr, x + 1)
        yl = min(yl, y)
        yr = max(yr, y + 1)
        zl = min(zl, z)
        zr = max(zr, z + 1)
    grundy_number ^= xl
    grundy_number ^= (X - xr)
    grundy_number ^= yl
    grundy_number ^= (Y - yr)
    grundy_number ^= zl
    grundy_number ^= (Z - zr)

print("LOSE" if grundy_number == 0 else "WIN")
```
</details>
    
<details>
    <summary>補足</summary>
    
特殊な制約でのゲーム問題は、<b>Grundy数</b>を求めることで勝者を求めることができる場合があります。その中でも Nim は容易に Grundy 数を求められることで有名であり、Nim に帰着させる問題も頻出です。
</details>
    
