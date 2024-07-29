<!--
author: HARADA Kento
-->
## [ARC090C Avoiding Collision](https://atcoder.jp/contests/arc090/tasks/arc090_c)

<details><summary> ヒント1 </summary>

まず、Sを始点とした場合とTを始点した場合のそれぞれについて、最短経路数の数え上げをする必要があります。

</details>

<details><summary> ヒント2 </summary>

最短経路数の数え上げはDijkstra法で最短経路を求めるのと同時に行うことができます。

</details>

<details><summary> ヒント3 </summary>

二人が出会わない最短経路の選び方を数え上げるのは困難です。
より簡単な問題に言い換えられないでしょうか。

</details>

<details><summary> ヒント4 </summary>

二人が出会う最短経路の選び方を数え上げ、全体から引き算しましょう。

</details>

<details><summary> 解説 </summary>
最短経路の数え上げについては、以下の記事を参照してください。
Dijkstra法により最短経路を求める場合とほぼ同じアルゴリズムにより数えることができます。

[最短経路の数え上げ](https://drken1215.hatenablog.com/entry/2018/02/09/003200)

二人が出会わない最短経路の数え上げは困難です。そこで、二人が出会う最短経路の選び方を数え上げ、全体から引き算しましょう。これは、「補集合を考える」という典型テクニックです。

重要な考察として、二人が最短経路を通る場合、二人が2回以上出会うことは無いというものがあります。このため、ある辺または頂点に注目し、「そこで二人が出会う最短経路の選び方の組の総数」を数えると、重複無く数えることができます。

「ある辺または頂点で二人が出会う最短経路の選び方の組の総数」は、S、Tそれぞれを始点とした場合の最短経路数を求めておくと、高速に求めることができます。

注目している辺または頂点が最短経路に使われているかチェックする必要があることに注意してください。


実装例 (C++)
```cpp
#include <bits/stdc++.h>
using namespace std;
#include <atcoder/modint>
using namespace atcoder;
using ll = long long;
const ll INF = 1e18;
using mint = modint1000000007;
using P = pair<ll,int>;

struct Edge{
    int to;
    ll w;
    Edge(int to,ll w):to(to),w(w) {}
};

int main(){
    int n,m,s,t;
    cin >> n >> m >> s >> t;
    s--,t--;
    vector<vector<Edge>> g(n);
    vector<int> u(m),v(m);
    vector<ll> d(m);
    for(int i = 0;i < m;i++){
        cin >> u[i] >> v[i] >> d[i];
        u[i]--,v[i]--;
        g[u[i]].emplace_back(v[i],d[i]);
        g[v[i]].emplace_back(u[i],d[i]);
    }
    auto dijkstra = [&](vector<ll> &dist,vector<mint> &cnt,ll start) -> void {
        dist[start] = 0;
        cnt[start] = 1;
        priority_queue<P,vector<P>,greater<P>> pq;
        pq.emplace(dist[start],start);
        while(!pq.empty()){
            auto [d,v] = pq.top();
            pq.pop();
            if(d > dist[v]) continue;
            for(auto e:g[v]){
                if(dist[e.to] == dist[v]+e.w){
                    cnt[e.to] += cnt[v];
                } else if(dist[e.to] > dist[v]+e.w){
                    dist[e.to] = dist[v]+e.w;
                    cnt[e.to] = cnt[v];
                    pq.emplace(dist[e.to],e.to);
                }
            }
        }
        return;
    };
    vector<ll> ds(n,INF),dt(n,INF);
    vector<mint> cs(n),ct(n);
    dijkstra(ds,cs,s);
    dijkstra(dt,ct,t);
    mint ans = cs[t]*ct[s];
    for(int i = 0;i < n;i++){
        if(ds[i] != dt[i]) continue;
        ans -= cs[i]*ct[i]*cs[i]*ct[i];
    }
    for(int e = 0;e < m;e++){
        ll i = u[e];
        ll j = v[e];
        if(ds[i]+d[e] != ds[j] && ds[j]+d[e] != ds[i]) continue;
        if(dt[i]+d[e] != dt[j] && dt[j]+d[e] != dt[i]) continue;
        if(ds[i]+dt[i] != dt[s]) continue;
        if(ds[j]+dt[j] != dt[s]) continue;
        if(ds[i] > ds[j]) swap(i,j);
        ll a = ds[i];
        ll b = ds[i]+d[e];
        ll x = dt[j];
        ll y = dt[j]+d[e];
        if(b <= x || y <= a) continue;
        ans -= cs[i]*ct[j]*cs[i]*ct[j];
    }
    cout << ans.val() << endl;
}
```

</details>