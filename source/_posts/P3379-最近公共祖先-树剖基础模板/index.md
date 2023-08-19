---
title: P3379 最近公共祖先 树剖基础模板
date: 2023-08-20 00:21:39
tags:
---

来不及解释了，直接上代码

```cpp
#include<bits/stdc++.h>
#define ll long long
#define N 500100
using namespace std;

ll n,m,s,x,y,a,b;
vector<ll> g[N];

bool vis[N];
ll fa[N],dep[N],siz[N],son[N];
ll top[N],tot,dfn[N],rnk[N];

inline void dfs1(ll root, ll depth){
	vis[root] = true;
	dep[root] = depth;
	ll maxSon = 0;
	for(auto soni : g[root]){
		if(vis[soni]) continue;
		fa[soni] = root;
		dfs1(soni, depth+1);
		siz[root] += siz[soni];
		if(siz[soni] > siz[maxSon]){
			maxSon = soni;
		}
	}
	siz[root]++;
	son[root] = maxSon;
}

inline void dfs2(ll root, ll topnow){
	++tot;
	dfn[root] = tot;
	rnk[tot] = root;
	top[root] = topnow;
	if(son[root] != 0) dfs2(son[root], topnow); // 要有重儿子才行，否则递归到0节点就死循环了
	for(auto soni : g[root]){
		if(soni == son[root] || soni == fa[root]) continue; // 注意要排掉两个点，父亲和重儿子
		dfs2(soni, soni);
	}
}

inline ll lca(ll a, ll b){
	while(top[a] != top[b]){
		if(dep[top[a]] < dep[top[b]]) swap(a, b);
		a = top[a];
		a = fa[a];
	}
	
	if(dep[a] < dep[b]) return a;
	else return b;
}

int main(){
//	freopen("ans.out", "w", stdout);
	cin >> n >> m >> s;
	for(int i = 1; i <= n-1; i++){
		cin >> x >> y;
		g[x].push_back(y);
		g[y].push_back(x);
	}
	
	dfs1(s, 1);
	
	dfs2(s, s);
	
	for(int i = 1; i <= m; i++){
		cin >> a >> b;
		cout << lca(a, b) << endl;
	}
	
	return 0;
}
```