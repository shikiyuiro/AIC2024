<!--
author: HARADA Kento
-->
## [ARC102E Stop. Otherwise...](https://atcoder.jp/contests/arc102/tasks/arc102_c)

<details><summary> ヒント1 </summary>

「どの異なる二つのサイコロの出目の和も $i$ にならない」という条件を、より考えやすい条件に言い換えてみましょう。

</details>

<details><summary> ヒント2 </summary>

$i$ が奇数のとき、「どの異なる二つのサイコロの出目の和も $i$ にならない」とは「$a+b = i$ となる $1 \leq a,b \leq K$ に対し、$(a,b)$ のどちらか一つは現れない」ということです。$i$ が偶数の場合はどうでしょうか。

</details>

<details><summary> ヒント3 </summary>

$i$ が偶数の場合は、ヒント2の条件に加え、「$K/2$ が $1$ 個以下しか現れない」という条件が加えられます。

</details>

<details><summary> ヒント4 </summary>

$a+b = i$ となるような $(a,b)$ の組の数を $p$ とします。$p$ 個の組の内、いくつかは両方とも現れず、残りは片方のみ現れます。片方のみ現れる組の数を全探索しましょう。

</details>

<details><summary> 解説 </summary>

$i$ が奇数の場合、偶数の場合について、満たすべき条件はヒント2、ヒント3の通りです。あとは、この条件を満たすような出目の組の場合の数が求められれば良いです。

$i$ が奇数の場合から考えます。$a+b = i$ となるような $(a,b)$ の組の数を $p$ とします。ヒント4でも述べたように、$p$ 個の組の内、いくつかは両方とも現れず、残りは片方のみ現れます。片方のみ現れる組の数が $q$ のとき、条件を満たす出目の組の場合の数は  $_{p}C_{q} \times 2^{q} \times _{K-2p+q}H_{N-q}$ 通りです。

$_pC_q$ は $p$ 個の組の中から $q$ 個を選ぶことに対応します。$2^{q}$ は選んだ $q$ 個の組それぞれについて $(a,b)$ のどちらを選ぶか決めることに対応しています。$_{K-2p+q}H_{N-q}$ の部分は、選ぶことのできる数 $K-2p+q$ 個の中から重複を許して $N-q$ 個を選ぶことに対応しています。

$i$ が偶数の場合は、 $K/2$ を選ぶか選ばないかで場合分けすることで、同様に解くことができます。

計算量は $\mathrm{O}(K^2+N)$です。

実装例（C++）
```cpp
#include <bits/stdc++.h>
using namespace std;
#include <atcoder/modint>
using namespace atcoder;
using mint = modint998244353;

// combination mod prime
// https://youtu.be/8uowVvQ_-Mo?t=6002
// https://youtu.be/Tgd_zLfRZOQ?t=9928
struct modinv {
  int n; vector<mint> d;
  modinv(): n(2), d({0,1}) {}
  mint operator()(int i) {
    while (n <= i) d.push_back(-d[mint::mod()%n]*(mint::mod()/n)), ++n;
    return d[i];
  }
  mint operator[](int i) const { return d[i];}
} invs;
struct modfact {
  int n; vector<mint> d;
  modfact(): n(2), d({1,1}) {}
  mint operator()(int i) {
    while (n <= i) d.push_back(d.back()*n), ++n;
    return d[i];
  }
  mint operator[](int i) const { return d[i];}
} facts;
struct modfactinv {
  int n; vector<mint> d;
  modfactinv(): n(2), d({1,1}) {}
  mint operator()(int i) {
    while (n <= i) d.push_back(d.back()*invs(n)), ++n;
    return d[i];
  }
  mint operator[](int i) const { return d[i];}
} ifacts;
mint comb(int n, int k) {
  if (n < k || k < 0) return 0;
  return facts(n)*ifacts(k)*ifacts(n-k);
}
mint homo(int n,int k){
    int x = n+k-1;
    int y = k;
    return comb(x,y);
}

int main(){
    int k,n;
    cin >> k >> n;
    for(int i = 2;i <= k*2;i++){
        mint ans;
        for(int q = 0;q <= (i-1)/2;q++){
            mint add;
            int p = min((i-1)/2,k-i/2);
            if(i%2){
                add += comb(p,q)*mint(2).pow(q)*homo(k-2*p+q,n-q);
            } else {
                add += comb(p,q)*mint(2).pow(q)*homo(k-2*p-1+q,n-q-1);
                add += comb(p,q)*mint(2).pow(q)*homo(k-2*p-1+q,n-q);
            }
            ans += add;
        }
        cout << ans.val() << endl;
    }
}
```

</details>

