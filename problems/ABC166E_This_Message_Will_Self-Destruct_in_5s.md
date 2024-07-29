<!--
author: HARADA Kento
-->
## [ABC166E This Message Will Self-Destruct in 5s](https://atcoder.jp/contests/abc166/tasks/abc166_e)

<details><summary> ヒント1 </summary>

「2人の持つ番号の差の絶対値が2人の身長の和に等しい」という条件を式で表してみましょう。
</details>

<details><summary> ヒント2 </summary>

参加者 $i,j (i<j)$ について、条件は $j-i=A_i+A_j$ と表せます。
この式を扱いやすい形に変形しましょう。

</details>

<details><summary> ヒント3 </summary>

ヒント2で示した式は、 $i+A_i=j-A_j$ と変形できます。

</details>

<details><summary> 解説 </summary>

条件が数式を用いない形で書かれている場合は、まず数式で表すのが定石です。  
「2人の持つ番号の差の絶対値が2人の身長の和に等しい」という条件は、「参加者 $i,j (i<j)$ について、 $j-i=A_i+A_j$ を満たす」と言い換えることができます。

このままでは扱いにくいので、 $i+A_i=j-A_j$ と変形します。添え字 $i,j$についての式を立てたとき、$i$ がある項を右辺に、$j$ がある項を左辺にまとめるのは典型テクニックです。

$L_i=i+A_i$、$R_i=i-A_i$ とおくと、条件はさらに $L_i=R_j$と書き換えられます。あとは取り得る値 $X$ を全て試し、$L_i=R_j=X$となるような $(i,j)$ の組の個数を数えれば良いです。  
$X$ を固定したとき、条件を満たす $(i,j)$ の組の個数は $(L_i=Xを満たすiの個数)\times(R_j=Xを満たすjの個数)$ で求めることができます。各 $X$ について $(L_i=Xを満たすiの個数)$ と $(R_j=Xを満たすjの個数)$を連想配列を使って求めておけば、上記の値を高速に計算することができます。計算量は $\mathrm{O}(\log N)$ です。

実装例 (C++)
```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n;
    cin >> n;
    vector<int> a(n);
    for(int i = 0;i < n;i++){
        cin >> a[i];
    }
    map<int,long long> l,r;
    for(int i = 0;i < n;i++){
        l[i+a[i]]++;
        r[i-a[i]]++;
    }
    long long ans = 0;
    for(auto [x,lcnt]:l){
        long long rcnt = r[x];
        ans += lcnt*rcnt;
    }
    cout << ans << endl;
}
```
実装例 (Python)
```python
from collections import defaultdict
n = int(input())
a = list(map(int, input().split()))
l = defaultdict(int)
r = defaultdict(int)

for i in range(n):
    l[i + a[i]] += 1
    r[i - a[i]] += 1

ans = 0
for x in l:
    ans += l[x] * r[x]

print(ans)
```

</details>
