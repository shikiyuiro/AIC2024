<!--
author: ISHIKAWA Yuichiro
-->
## [ARC111A Simple Math 2](https://atcoder.jp/contests/arc111/tasks/arc111_a)

<details><summary> ヒント </summary>

ある種の整数問題では、商とあまりの関係式に落とし込んで考えると見通しが良くなることがあります。

たとえば、

$10^N = Q * M + R \quad (0 \leq R \lt M)$

$Q = q * M + r  \quad (0 \leq r \lt M)$

とおくとき、<br>
問題文で与えられた $\lfloor \frac{10^N}{M} \rfloor$ は上の $Q$ に、それをMで割ったあまりは $r$ にそれぞれ対応しています。

上の2つの式を用いて $r$ を求めやすい形に表してみましょう。

</details>

<details><summary> さらにヒント </summary>

ふたつの関係式について、一つ目の式の $Q$ に二つ目の式を代入してみましょう。

すると、

$10^N = (q * M + r) * M + R$

整理して、関係式

$10^N = q * M^2 + r * M + R$

が得られます。<br>
この式をよく見てみると、 $10^N$ を $M^2$ で割ったあまりが $r * M + R$ ,
さらに $r * M + R$ を $M$ で割ったあまりがちょうど $r$ に対応していると捉えることができます。

したがって、求める値は「 $10^N$ を $M^2$ で割ったあまりを $M$ で割った時の商」に等しいです。

(注意)  
・C++の場合、$10^N$ が大きすぎてこのままでは直接計算できません。繰り返し二乗法を用いましょう。 [繰り返し二乗法とは?](https://algo-logic.info/calc-pow/)  
・Pythonの場合、繰り返し二乗法が標準のpow関数に実装されています。

</details>

<details><summary> 解説 </summary>

求める値は「 $10^N$ を $M^2$ で割ったあまりを $M$ で割った時の商」に等しいです。考え方のポイントは上のヒントを参考にしてください。

実装例 (Python)
```python
N, M = map(int, input().split())
Q = pow(10, N, M*M)
print(Q // M)

```

実装例 (C++)
```cpp
#include<bits/stdc++.h>
using namespace std;

int main(){
    int64_t res = 1;
    int64_t N, M; cin >> N >> M;
    int64_t M2 = M*M;
    vector<int64_t> dp(61); // dp[i] := 10^(2^i)をM^2で割ったあまり
    dp[0] = 10 % M2;
    for(int64_t i = 1; i < 61; i++) dp[i] = dp[i-1] * dp[i-1] % M2;
    for(int64_t i = 0; i < 61; i++){
        if((N >> i)&1){
            res *= dp[i];
            res %= M2;
        }
    }
    res /= M;
    cout << res << '\n';
}

```

</details>
