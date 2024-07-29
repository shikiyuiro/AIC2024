<!--
author: ISHIKAWA Yuichiro
-->
## [ABC121D XOR World](https://atcoder.jp/contests/abc121/tasks/abc121_d)

<details><summary> ヒント </summary>

$f(A, B) = f(0, B) \; \mathrm{xor} \;  f(0, A-1)$  
と変形できることに注意しましょう。  
小さい値$x$で、$f(0,x)$を実験してみると、どのようになるでしょうか。

</details>

<details><summary> 解説 </summary>

$f(0, x)$を実験すると、$x$が$4$でわって$3$あまるとき$f(0, x) = 0$ となることが予想できます。(たとえば$f(0, 7) = 0$)  
これは、次のようにして示すことができます。  
いま、任意の非負整数$n$について、  
$4n \; \mathrm{xor} \;  4n+1  \; \mathrm{xor} \; 4n+2  \; \mathrm{xor} \; 4n+3 = 0$  
を示せば、それらを組み合わせることで証明できます。  
(証明)  
$4n \; \mathrm{and} \; 0 = 0$  
$4n \; \mathrm{and} \; 1 = 0$  
$4n \; \mathrm{and} \; 2 = 0$  
$4n \; \mathrm{and} \; 3 = 0$  
であるので、$\mathrm{xor}$と和の関係から、   
$4n + 0 = 4n \; \mathrm{xor} \; 0$  
$4n + 1 = 4n \; \mathrm{xor} \; 1$  
$4n + 2 = 4n \; \mathrm{xor} \; 2$  
$4n + 3 = 4n \; \mathrm{xor} \; 3$  
が成り立ち、これより  
$4n \; \mathrm{xor} \;  4n+1  \; \mathrm{xor} \; 4n+2  \; \mathrm{xor} \; 4n+3 \\ = 4n \; \mathrm{xor} \;  4n\; \mathrm{xor} \;1  \; \mathrm{xor} \; 4n\; \mathrm{xor} \;2  \; \mathrm{xor} \; 4n\; \mathrm{xor} \;3 \\ = 0 \; \mathrm{xor} \;  1  \; \mathrm{xor} \; 2  \; \mathrm{xor} \; 3 \\ = 0$  
が導かれます。  
(証了)  
この結果から、任意の$f(0, x)$を高速に求めることができる(たとえば、$f(0, 9) = f(0, 7) \; \mathrm{xor} \; 8 \; \mathrm{xor} \; 9$ とすればよい)ため、問題が解けました。
</details>
