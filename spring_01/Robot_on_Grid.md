## Robot on Grid
[問題リンク](https://atcoder.jp/contests/keyence2021/tasks/keyence2021_c)

<details><summary> ヒント1 </summary>

もしも空白マスが無い場合、どのような解法で解けるでしょうか。

</details>

<details><summary> ヒント2 </summary>

空白マスがある場合は、同じ経路が答えに複数回加算されます。ある経路が答えに何回加算されるか考えましょう。

</details>

<details><summary> ヒント3 </summary>

各経路は、「そのような移動が可能な書き込み方の総数」分だけ答えに加算されます。これを求めるにはどのような情報が必要でしょうか。

</details>

<details><summary> ヒント4 </summary>

$\mathrm{\Theta}(HW(H+W))$の解法を考え、それを高速化してみましょう。

</details>

<details><summary> 解説 </summary>

もしも空白マスが無い場合は、DPで解くことができます（有名問題です）。
今回解きたい問題はこの有名問題を含んでいるため、似た解法が使えないか考えます。  
これは、「解きたい問題が有名問題を含んでいるとき、それと同様の解法が使えないか考える」という典型テクニックです。[この考え方を使う問題の例](https://atcoder.jp/contests/arc159/tasks/arc159_d)

マスへの書き込み方全てに対してDPで経路数を求めていると時間が足りません。  
そこで、「ある
$(1,1)$
から
$(H,W)$
までの経路について、そのような移動が可能な書き込み方の総数」を全ての経路について求め、足し合わせることを考えましょう。これは主客転倒と呼ばれる典型テクニックです。

ある経路に注目するとき、その経路で移動が可能な書き込み方の総数を計算するためには「その経路に含まれる空白マスの個数」が分かれば良いです。  
通った経路に含まれる空白マスを
$t$
個とすると、その経路で移動が可能な書き込み方の総数は
$2^t \times 3^{K-t}$
通りです。これは、通った空白マスへの書き込み方が2通り、通っていない空白マスへが書き込み方は3通りあるためです。

「グリッド上の経路数を求めるDPに似たものを使う」ことと、「各経路については空白マスが含まれる個数のみ分かればよい」ということを合わせると、DPテーブルの定義として以下のものが思いつきます。  
$dp[i][j][t] = ((1,1)から出発して空白マスをt箇所通って(i,j)に到達するような経路の個数)$  
求める答えは
$\sum_{1\leq t \leq H+W} dp[H][W][t] \times 2^t 3^{HW-K-t}$
です。  
この解法を用いると
$\mathrm{\Theta}(HW(H+W))$
で解くことができますが、このままだとTLEします。
高速化するにはどうすれば良いでしょうか？（計算量の悪い解法から考えて、それを高速化するという典型テクニックです）

先程の式は  
$\sum_{1\leq t \leq H+W} dp[H][W][t] \times 2^t 3^{HW-K-t} = 3^{HW-K} \times  \sum_{1\leq t \leq H+W} dp[H][W][t] \times (2/3)^t$  
のように分解することができます。  
この式の  
$\sum_{1\leq t \leq H+W} dp[H][W][t] \times (2/3)^t$  
の部分は、添え字
$t$
を持つ代わりに、空白マスを通るたび
$(2/3)$
倍して遷移させることでDP配列の次元を落とすことができます。  
以上の解法により、この問題を$\mathrm{O}(HW)$で解くことができます。

実装例 (C++)
```cpp
#include <bits/stdc++.h>
using namespace std;
#include <atcoder/modint>
using namespace atcoder;
using mint = modint998244353;

vector<int> di = {1,0};
vector<int> dj = {0,1};

int main(){
    int h,w,k;
    cin >> h >> w >> k;
    vector<vector<int>> board(h,vector<int>(w,-1));
    //-1:空白 0:R 1:D 2:X
    for(int i = 0;i < k;i++){
        int hi,wi;
        char ci;
        cin >> hi >> wi >> ci;
        hi--,wi--;
        if(ci == 'R') board[hi][wi] = 0;
        if(ci == 'D') board[hi][wi] = 1;
        if(ci == 'X') board[hi][wi] = 2;
    }
    vector<vector<mint>> dp(h,vector<mint>(w));
    dp[0][0] = 1;
    mint c23 = mint(2)/3;
    for(int i = 0;i < h;i++){
        for(int j = 0;j < w;j++){
            for(int v = 0;v < 2;v++){
                if(v == 0 && board[i][j] == 0) continue;
                if(v == 1 && board[i][j] == 1) continue;
                int ni = i+di[v];
                int nj = j+dj[v];
                if(ni < 0 || nj < 0 || ni >= h || nj >= w) continue;
                mint coef = 1;
                if(board[i][j] == -1){
                    coef = c23;
                }
                dp[ni][nj] += dp[i][j]*coef;
            }
        }
    }
    mint ans = dp[h-1][w-1];
    ans *= mint(3).pow(h*w-k);
    cout << ans.val() << endl;
}
```

</details>