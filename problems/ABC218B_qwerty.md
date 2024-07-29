<!--
author: ISHIKAWA Yuichiro
-->
## [ABC218B qwerty](https://atcoder.jp/contests/abc218/tasks/abc218_b)

<details><summary> ヒント </summary>

数を文字に変換する方法を習得しましょう！<br>さまざまな方法がありますが、<br>

・c++では ```(char)('a' + i)```<br>
・pythonでは ``` char(97 + i) ```<br>

によって、 'a' の $i$ 個先の英小文字が得られます。

</details>

<details><summary> 解説 </summary>

・c++では ```(char)('a' + i)```<br>
・pythonでは ``` char(97 + i) ```<br>

によって、 'a' の $i$ 個先の英小文字が得られます。

実装例 (Python)
```python
P = list(map(int, input().split()))
res = ""
for p in P:
    res += chr(97 + p-1)
print(res)

```

実装例 (C++)
```cpp
#include<bits/stdc++.h>
using namespace std;

int main(){
    string res = "";
    for(int i = 0; i < 26; i++){
        int p; cin >> p;
        res += char('a' + p-1);
    }
    cout << res << '\n';
}

```

</details>
