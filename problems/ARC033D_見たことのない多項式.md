<!--
author: SASAKI Yuma
-->
## [ARC33D 見たことのない多項式](https://atcoder.jp/contests/arc033/tasks/arc033_4)

<details><summary> ヒント１ </summary>
    
条件を満たす $N$ 次多項式は一意に存在するので，これを見つける方法を考えてみましょう（知識要素が大きいので，ピンと来なければ調べてみるといいと思います）．
    
</details>

<details><summary> ヒント２ </summary>
    
ヒント１が分からなければ，ラグランジュ補間で調べてみましょう．
    
</details>

<details><summary> ヒント３ </summary>
    
計算するべき式で，高速化できる部分がないか考えてみましょう．
</details>

<details><summary> 解説</summary>
    
この問題は解説がかなり詳しいので，詳しい解法についてはそちらを参照してみてください．
    
また，この問題は与えられる点（の $x$ 座標）が等差数列であることを用いた高速化が想定解になっていますが，そうでない場合もある程度高速に多項式を復元することが可能です（ $O(N ({\log N})^2$ [yosupo judge](https://judge.yosupo.jp/problem/polynomial_interpolation))．
    
[参考](https://37zigen.com/lagrange-interpolation/#i-2)

</details>
