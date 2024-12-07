## How many?
[問題リンク](https://atcoder.jp/contests/abc214/tasks/abc214_b)

<details><summary> ヒント1 </summary>

全探索をしたいですが、
$a,b,c$
をそれぞれいくつまで探索すればよいでしょうか。
それぞれ
$10000$
まで試していては間に合いません。

</details>

<details><summary> ヒント2 </summary>

$a+b+c \leq S$
と、
$(a,b,c)$
が非負整数であるという条件から、探索範囲を絞ることができます。

</details>

<details><summary> 解説 </summary>

非負整数の組
$(a,b,c)$
が
$a+b+c \leq S$
を満たすなら、
$a \leq S$
かつ
$b \leq S$
かつ
$c \leq S$
です。
よって、この範囲で
$(a,b,c)$
を全探索し、二つの条件を満たすか調べれば良いです。  
これは、「探索範囲を絞る」という典型テクニックです。

計算量は
$\mathrm{O}(S^3)$
で、
$S \leq 100$
であるため十分高速に動作します。

実装例 (Python)
```python
s,t = map(int,input().split())
ans = 0
for i in range(0,101):
    for j in range(0,101):
        for k in range(0,101):
            if i+j+k <= s and i*j*k <= t:
                ans += 1
print(ans)
```

実装例 (C++)
```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
    int s,t;
    cin >> s >> t;
    int ans = 0;
    for(int i = 0;i <= s;i++){
        for(int j = 0;j <= s;j++){
            for(int k = 0;k <= s;k++){
                if(i+j+k <= s && i*j*k <= t){
                    ans++;
                }
            }
        }
    }
    cout << ans <<endl;
}
```

</details>