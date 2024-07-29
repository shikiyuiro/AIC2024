<!--
author: SASAKI Yuma
-->
## [ARC065E へんなコンパス](https://atcoder.jp/contests/arc065/tasks/arc065_c)

<details><summary>記法について</summary>
    
問題文では点 $i$ や $j$ と書かれていますが，ヒントや解説ではこれと同じ意味で点 $P_i$ や $P_j$ と表記しています．
</details>
    
<details><summary>ヒント1</summary>

コンパスが点 $P_i$ を指す可能性があるかどうかを求める方法を考えてみよう．
</details>
    
<details><summary>ヒント2</summary>

各点を頂点とするグラフで，マンハッタン距離が $d(P_a,P_b)$ と等しいものに辺を貼ったグラフを $G$ とするとき，
コンパスが点 $P_i$ を指す可能性がある $\Leftrightarrow$ $G$ において点 $P_i$ と点 $P_a$ に対応する頂点が同じ連結成分にある
</details>
    
<details><summary>ヒント3</summary>

$45$ 度回転を考えよう（初見ならばググる or 解説スライドのこの部分まで見てみてください）．
</details>
    
<details><summary>ヒント4</summary>

コンパスが指す片方の点が $P_i$ であるとき，もう片方の点としてありうるものの個数を求める方法を考えよう．
</details>
    
<details><summary> 解説 </summary>
    
[URL](https://drive.google.com/file/d/10IL93M1MEsI4zKlg2_fDTLeaw2tu-IhE/view)
    
<iframe src="https://drive.google.com/file/d/10IL93M1MEsI4zKlg2_fDTLeaw2tu-IhE/preview" width="800" height="500"　allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
    
</details>
    
