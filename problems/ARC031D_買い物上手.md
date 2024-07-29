<!--
author: SASAKI Yuma
-->
## [ARC031D 買い物上手](https://atcoder.jp/contests/arc031/tasks/arc031_4)
    
<details><summary>ヒント1</summary>
    
二分探索します．割合の最大化でよく見る典型です．判定問題をどのように解けるか考えましょう．
</details>

<details><summary>ヒント2</summary>
    
判定問題を定式化します．答えとして $k$ 以上を達成可能である条件は，
\begin{align*}
\text{経験値} - k \times \text{お金} \geq 0
\end{align*}
となるような買い方が存在することです．

</details>

<details><summary>ヒント3</summary>
    
判定問題をより定式化します．まず，全ての商品の値段を $k$ 倍することで，単に $\text{経験値} - \text{お金} \geq 0$ とできるかを考えればよいです．
また，この問題は各アイテムについて，買う or 買わないを選択する問題だと考えることができます．つまり，
- $1 \leq i \leq M$ について， $a_i \in \{0,1\}$ を定める．
- 各 $1 \leq i \leq N$ について， $a_{A_{i,1}} = a_{A_{i,2}} = \cdots = a_{A_{i,K_i}} = 1$ の場合に限り，経験値 $S_i$ を得る．
- 使うお金の総和は $\sum_{a_i = 1} T_i$
- このような条件の下で， $\text{経験値}-\text{お金}$ を最大化しなさい
    
と整理することができます．

</details>

<details><summary>ヒント4</summary>
    
ヒント 3 の条件をよく考えると， PSP（燃やす埋める問題）であることが分かります．
</details>
    
<details><summary> 解説 </summary>
    
[URL](https://drive.google.com/file/d/13fcMs1w_gCjdB5Gb6tMo7SHJna-aqxBU/view)
    
<iframe src="https://drive.google.com/file/d/13fcMs1w_gCjdB5Gb6tMo7SHJna-aqxBU/preview" width="800" height="500"　allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
    
</details>
