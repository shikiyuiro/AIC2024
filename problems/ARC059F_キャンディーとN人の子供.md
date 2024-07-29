<!--
author: HARADA Kento
-->
## [ARC059F キャンディーとN人の子供](https://atcoder.jp/contests/arc059/tasks/arc059_c)

<details><summary> ヒント+本解説について </summary>

DPで解く方針とFPSで解く方針があります。DP解法については公式解説で説明されているので、本解説ではFPSで説明します。FPSに慣れていない方は[maspyさんの記事](https://maspypy.com/%E5%A4%9A%E9%A0%85%E5%BC%8F%E3%83%BB%E5%BD%A2%E5%BC%8F%E7%9A%84%E3%81%B9%E3%81%8D%E7%B4%9A%E6%95%B0%E6%95%B0%E3%81%88%E4%B8%8A%E3%81%92%E3%81%A8%E3%81%AE%E5%AF%BE%E5%BF%9C%E4%BB%98%E3%81%91)を参照してください。  
まずは部分点解法を考えましょう。それを少し改造すると満点解法になります。

</details>

<details><summary> 解説 </summary>

本解説では、「$f$ の $x^n$ の係数」を $[x^n]f$ と表記します。

まずは部分点解法を考えます。部分点の制約下では子供たちのはしゃぎ度が固定されています。子供 $i$ のはしゃぎ度を $h_i (= A_i = B_i)$ としましょう。  
すると、答えは $[x^c](\prod_{i=1}^N(\sum_{j=0}^Ch_i^jx^j))$ と表されます。  
等比数列の和の公式を用いると、この式は $[x^c](\prod_{i=1}^N(\frac{1-h_i^{c+1}x^{c+1}}{1-h_ix}))$ と変形することができます。これを計算すると、部分点を取ることができます。  
[FPS部分の引用元](https://web.archive.org/web/20220813112459/https://opt-cp.com/fps-problem-list/)  
[実装例(部分点)](https://atcoder.jp/contests/arc059/submissions/53788222)

さて、満点解法を考えます。結論から述べると、本問題の答えは$[x^c](\prod_{i=1}^N(\sum_{h=a_i}^{b_i}(\sum_{j=0}^Ch^jx^j)))$ と表されます。部分点解法の式に、$\sum_{h=a_i}^{b_i}$ を挿入した形です。これが腑に落ちない場合は、[maspyさんの記事](https://maspypy.com/%E5%A4%9A%E9%A0%85%E5%BC%8F%E3%83%BB%E5%BD%A2%E5%BC%8F%E7%9A%84%E3%81%B9%E3%81%8D%E7%B4%9A%E6%95%B0%E6%95%B0%E3%81%88%E4%B8%8A%E3%81%92%E3%81%A8%E3%81%AE%E5%AF%BE%E5%BF%9C%E4%BB%98%E3%81%91)で立式の練習をすると良いと思います。部分点の場合と同様に、$[x^c](\prod_{i=1}^N(\sum_{h=a_i}^{b_i}\frac{1-h^{c+1}x^{c+1}}{1-hx}))$ と変形することができ、これを計算すると満点を取ることができます。  
[実装例(満点)](https://atcoder.jp/contests/arc059/submissions/53788783)

</details>
