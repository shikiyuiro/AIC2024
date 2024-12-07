## MAD TEAM
[問題リンク](https://atcoder.jp/contests/zone2021/tasks/zone2021_c)

<details><summary> ヒント1 </summary>

最小値の最大化なので、二分探索が使えないか考えましょう。

</details>

<details><summary> ヒント2 </summary>

「チームの総合力を
$x$
以上にできるか？」という判定問題を考えましょう。

</details>

<details><summary> ヒント3 </summary>

能力値の種類数が少ないことに注目しましょう。

</details>

<details><summary> 解説 </summary>
チームメンバーの選び方を全て試していては時間が足りません。これは、メンバー候補の数が多いことが原因なので、どうにかしてメンバー候補の数を減らしたいです。

そこで、答えを二分探索します。最小値の最大化や最大値の最小化をする場合は、二分探索が有効であることが多いです。

「チームの総合力を
$x$
以上にできるか？」という判定問題を考えます。  
判定問題を考える上では、各メンバーの能力値が
$x$
以上か、
$x$
未満かという情報のみあれば十分です。よって、各メンバーの能力値を、
$x$
以上の場合は
$1$
、
$x$
未満の場合は
$0$
という具合に圧縮することができます。これは、「ある値との大小のみが重要な場合は2値に変換できる」という典型テクニックです。  [この考え方を使う問題](https://atcoder.jp/contests/abc107/tasks/arc101_b)
  
以上の考察を踏まえると、能力値を01に圧縮した後は能力値の組としてあり得るものが
$2^5 = 32$
種類しか無いことが分かります。

例えば、チームの総合力を5以上にできるか？という判定問題を考える際は、  
$(A,B,C,D,E) = (1,6,2,2,7)$  
の人と  
$(A,B,C,D,E) = (1,7,1,3,9)$  
の人はどちらも圧縮すると  
$(A',B',C',D',E') = (0,1,0,0,1)$  
になるため区別する必要がありません。

このように考えると、圧縮後の能力値が一致するメンバーを取り除くことができます。従って、メンバー候補の数が多いという課題が解消され、メンバーの選び方を全探索できるようになります。全探索により判定問題が解けるので、この問題を解くことができました。計算量は、能力値の種類を
$M$
、採用する人数を
$K$
、二分探索の上限を
$X$
として、
$\mathrm{O}((NM+2^{MK})+\log(X))$
です。


実装例 (C++)
```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    int n;
    cin >> n;
    const int m = 5;
    const int m2 = 1<<m;
    vector<vector<int>> a(n,vector<int>(m));
    for(int i = 0;i < n;i++){
        for(int j = 0;j < m;j++){
            cin >> a[i][j];
        }
    }
    auto f = [&](int x){
        vector<int> exist(m2);
        for(int i = 0;i < n;i++){
            int bit = 0;
            for(int j = 0;j < m;j++){
                if(x <= a[i][j]) bit |= 1<<j;
            }
            exist[bit] = 1;
        }
        for(int i = 0;i < m2;i++){
            if(exist[i] == 0) continue;
            for(int j = i;j < m2;j++){
                if(exist[j] == 0) continue;
                for(int k = j;k < m2;k++){
                    if(exist[k] == 0) continue;
                    if(((i|j)|k) == m2-1) return true;
                }
            }
        }
        return false;
    };
    int ok = 0,ng = 1e9+10;
    while(abs(ok-ng)>1){
        int mid = (ok+ng)/2;
        if(f(mid)) ok = mid;
        else ng = mid;
    }
    cout << ok << endl;
}
```

</details>

<details><summary>補足</summary>
この問題における重要な典型テクニックとして、「問題で与えられる定数に注目する」というものもあります。

今回の問題では、能力値の種類数が5種類（定数）でした。このような場合は、その数が小さいことや、性質が良いことを利用できる場合があります。

この問題では、能力値の種類数が少ないことに注目すると、2値に圧縮して組み合わせを全探索できないか？というアイディアを思いつくことができます。

- [数が小さいことを利用する問題](https://onlinejudge.u-aizu.ac.jp/-challenges/sources/ICPC/Prelim/1651?year=2021)  
- [数の性質が良いことを利用する問題](https://atcoder.jp/contests/abc135/tasks/abc135_d)
</details>