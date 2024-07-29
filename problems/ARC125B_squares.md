<!--
author: ISHIKAWA Yuichiro
-->
## [ARC125B squares](https://atcoder.jp/contests/arc125/tasks/arc125_b)

<details><summary> ヒント </summary>

$z^2 = x^2 - y$としましょう。  
$x, z^2$が定まれば$y$は一意に定まるので、与えられた条件は以下のようになります。  
・ $1 \leq x \leq N$  
・ $0 \leq z$  
・ $x^2 - z^2$は$1$以上$N$以下の整数である

$x^2 - z^2 = (x + z)(x - z)$に注意しましょう。

</details>

<details><summary> 解説 </summary>

$z^2 = x^2 - y$としましょう。  
$x, z^2$が定まれば$y$は一意に定まるので、与えられた条件は以下のようになります。   
・ $1 \leq x \leq N$  
・ $0 \leq z$  
・ $x^2 - z^2$は$1$以上$N$以下の整数である

ここで、$x^2 - z^2 = (x + z)(x - z)$より、$(x+z), (x-z)$を数え上げる問題に帰着させます。(特に、$x+z, x-z$が固定されたとき、$x, z$の値は一意に定ります。)

改めて、$p = (x+z), q = (x-z)$とおくと、条件は以下のようになります。  
・ $1 \leq x = (p+q)/2 \leq N$  
・ $0 \leq z = (p-q)/2$  
・ $x^2 - z^2 = pq$は$1$以上$N$以下の整数である  
(3番目の条件が満たされれば、つねに $(p+q)/2 \leq N$が成り立ちます)

$p, q$の動く範囲を考えます。$0 \leq z = (p-q)/2 $ より、$q \leq p$に注意すると、  
$q$は$1, 2, ..., \lfloor\sqrt{N}\rfloor$まで調べればよいです。  
このとき、$p$の動く範囲は  
・1番目の条件より$2 - q \leq p$  (この条件は$q \leq 1$より実は不要)  
・3番目の条件より$1 \leq p \leq N/q$  
・$2z = (p-q)$より、$p$と$q$の偶奇は一致する  
であるため、  
時間計算量 $\mathrm{O}(\sqrt{N})$ で調べることができます。

</details>
