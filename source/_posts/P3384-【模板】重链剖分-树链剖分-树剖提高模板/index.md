---
title: P3384 【模板】重链剖分/树链剖分 树剖提高模板
date: 2023-08-20 00:24:16
tags:
---

# 题目来源

[P3384 【模板】重链剖分/树链剖分](https://www.luogu.com.cn/problem/P3384)

# 题意简述

已知一棵包含 $N$ 个结点的树（连通且无环），每个节点上包含一个数值，需要支持以下操作：

* `1 x y z`，表示将树从 $x$ 到 $y$ 结点最短路径上所有节点的值都加上 $z$。

* `2 x y`，表示求树从 $x$ 到 $y$ 结点最短路径上所有节点的值之和。

* `3 x z`，表示将以 $x$ 为根节点的子树内所有节点值都加上 $z$。

* `4 x` 表示求以 $x$ 为根节点的子树内所有节点值之和

$1\le N \leq {10}^5$，$1\le M \leq {10}^5$，$1\le R\le N$，$1\le P \le 2^{31}-1$。

# 解题思路

树剖。

太板，不解释。

# 注意事项

处处都要取模！

第一次提交时 case 11 被卡，原因是是建树的时候给 $t[root].v$ 赋值时只赋值没有取模，导致对于孤点的情况，流程中没有其他机会取模。若此时模数又等于孤点上的数字，就会被卡。

线段树标记下传的时候，更新子节点 `val` 域使用的因子只能是父节点的就标记，而不是子节点的新标记。因为子节点的标记中包含了之前的存量，直接拿来用相当于把之前所有的标记又给子节点重复打了一次。

# AC代码

```cpp
#include<bits/stdc++.h>
#define ll long long
#define N 100100
using namespace std;

struct sgt{
	ll p,l,r,ls,rs,v,t;
}t[N << 3];

ll n,m,r,p;
ll a[N];
vector<ll> g[N];
ll op,x,y,z;

bool vis[N];
ll fa[N],dep[N],siz[N],son[N];
ll top[N],tot,dfn[N],rkn[N],bottom[N];

inline void dfs1(ll root, ll nowdep){
	vis[root] = true;
	dep[root] = nowdep;
	ll maxSon = 0;
	for(auto soni : g[root]){
		if(vis[soni]) continue;
		fa[soni] = root;
		dfs1(soni, nowdep+1);
		siz[root] += siz[soni];
		if(siz[soni] > siz[maxSon]){
			maxSon = soni;
		}
	}
	siz[root]++;
	son[root] = maxSon;
}

inline ll dfs2(ll root, ll nowtop){
	top[root] = nowtop;
	++tot;
	dfn[root] = tot;
	rkn[tot] = root;
	ll nowbottom = root;
	if(son[root] != 0) nowbottom = dfs2(son[root], nowtop); // nowbottom在这里也必须更新
	for(auto soni : g[root]){
		if(soni == son[root] || soni == fa[root]) continue;
		nowbottom = dfs2(soni, soni);
	}
	bottom[root] = nowbottom;
	return nowbottom;
}

inline void build(ll root, ll l, ll r){
	t[root].p = root, t[root].l = l, t[root].r = r;
	if(l == r){
		t[root].v = a[rkn[l]];
		t[root].v %= p; // 要加这一行，否则孤点的情况下没有其他机会取模，会被卡
		return;
	}
	t[root].ls = root*2, t[root].rs = root*2 + 1;
	ll ls = t[root].ls, rs = t[root].rs;
	ll mid = (l + r) >> 1;
	build(ls, l, mid);
	build(rs, mid+1, r);
	t[root].v = t[ls].v + t[rs].v;
	t[root].v %= p; // 这个地方也要取模
}

inline void pushdown(ll root){
	ll l = t[root].l, r = t[root].r;
	ll ls = t[root].ls, rs = t[root].rs;
	ll mid = (l + r) >> 1;
	t[ls].t += t[root].t, t[ls].t %= p;
	t[ls].v += (mid-l+1) * t[root].t, t[ls].v %= p; 
	// 警告：这里只能用t[root].t作为增量的因子，若用t[ls]t则错误，因为后者有存量
	t[rs].t += t[root].t, t[rs].t %= p;
	t[rs].v += (r-mid) * t[root].t, t[rs].v %= p;
	t[root].t = 0;
}

inline void modify(ll root, ll x, ll y, ll z){
	ll l = t[root].l, r = t[root].r;
	ll ls = t[root].ls, rs = t[root].rs;
	if(x<=l && r<=y){
		t[root].t += z, t[root].t %= p;
		t[root].v += (r-l+1) * z, t[root].v %= p;
		return;
	}
	if(x>r || y<l) return;
	ll mid = (l + r) >> 1;
	pushdown(root);
	if(x <= mid) modify(ls, x, y, z);
	if(y >= mid+1) modify(rs, x, y, z);
	t[root].v = (t[ls].v + t[rs].v) % p;
	return;
}

inline ll query(ll root, ll x, ll y){
	ll l = t[root].l, r = t[root].r;
	ll ls = t[root].ls, rs = t[root].rs;
	if(x<=l && r<=y) return t[root].v;
	if(x>r || y<l) return 0;
	ll mid = (l + r) >> 1;
	ll ans = 0;
	pushdown(root);
	if(x <= mid) ans += query(ls, x, y);
	if(y >= mid+1) ans += query(rs, x, y);
	ans %= p;
	return ans;
}

inline void modifyPath(ll x, ll y, ll z){
	while(top[x] != top[y]){
		if(dep[top[x]] < dep[top[y]]) swap(x, y);
		ll nowtop = top[x];
		modify(1, dfn[nowtop], dfn[x], z);
		x = fa[nowtop];
	}
	if(dep[x] > dep[y]) swap(x, y);
	modify(1, dfn[x], dfn[y], z);
}

inline ll queryPath(ll x, ll y){
	ll ans = 0;
	while(top[x] != top[y]){
		if(dep[top[x]] < dep[top[y]]) swap(x, y);
		ll nowtop = top[x];
		ans += query(1, dfn[nowtop], dfn[x]);
		ans %= p;
		x = fa[nowtop];
	}
	if(dep[x] > dep[y]) swap(x, y);
	ans += query(1, dfn[x], dfn[y]);
	ans %= p;
	return ans;
}

inline void modifyTree(ll x, ll z){
	modify(1, dfn[x], dfn[bottom[x]], z);
}

inline ll queryTree(ll x){
	return query(1, dfn[x], dfn[bottom[x]]);
}

int main(){
//	freopen("data.in", "r", stdin);
//	freopen("ans.out", "w", stdout);
	cin >> n >> m >> r >> p;
	for(int i = 1; i <= n; i++){
		cin >> a[i];
	}
	for(int i = 1; i <= n-1; i++){
		cin >> x >> y;
		g[x].push_back(y);
		g[y].push_back(x);
	}
	
	dfs1(r, 1);
	
	dfs2(r, r);
	
	build(1, 1, n);
	
	for(int i = 1; i <= m; i++){
		cin >> op;
		if(op == 1){
			cin >> x >> y >> z;
			modifyPath(x, y, z);
		}
		else if(op == 2){
			cin >> x >> y;
			cout << queryPath(x, y) << endl;
		}
		else if(op == 3){
			cin >> x >> z;
			modifyTree(x, z);
		}
		else{
			cin >> x;
			cout << queryTree(x) << endl;
		}
	}
	
	return 0;
}
```

# 后记

当绿色的 <font color=green> Accepted </font> 在屏幕上亮起时，

我突然回想起 OI 退役的前一天晚上，我就在酒店里写这个题目。

可惜写出来没有调完，时间太晚就睡觉了。

第二天匆匆退役。

从 2020 年 12 月 4 日，到现在 2023 年 8 月 20 日的凌晨，

与此题阔别将近三年时间。

这三年发生了太多的事情，见证了悲欢离合，甚至是生离死别。

感受了温情、欣喜、庆幸、感动、解脱，

也经历过失意、无奈、痛苦、迷茫、悔恨。

当初陪在我身边的人，如今已经和我渐行渐远，

当初我的很多想法，如今已经发生了很大改变。

当量变引起质变，我也不得不承认现在的自己已经不是曾经的自己了。

这样的回忆也引发了我更多的思考，关于生活，关于家庭，关于事业，关于人生……

当然，我也很快回过神来，因为在深夜搞这样的胡思乱想不可能会有什么成果，只会白白浪费宝贵的睡觉时间。

家庭的负担、社会的复杂、身体的疲惫、精神的空虚、灵魂的麻木……

当绿色的 <font color=green> Accepted </font> 在屏幕上亮起的那一瞬间，这些事情通通都可以抛之脑后，我只需要看着那个绿色的 <font color=green> Accepted </font> ，姑且享受一瞬间的快乐。

虽然只有一瞬间，但至少也还有一瞬间。

那一瞬间，是专属于一个 OIer 的，专属于一个长不大的男孩的，一份孤独而平静的快乐。