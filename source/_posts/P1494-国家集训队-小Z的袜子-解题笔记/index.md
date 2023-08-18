---
title: P1494 [国家集训队] 小Z的袜子 解题笔记
date: 2023-08-18 23:26:22
tags:
---

# 题目来源

[P1494 [国家集训队] 小 Z 的袜子](https://www.luogu.com.cn/problem/P1494)

# 题意简述

有一个长度为 $n$ 的序列 $\{c_i\}$。现在给出 $m$ 个询问，每次给出两个数 $l,r$，从编号在 $l$ 到 $r$ 之间的数中随机选出两个不同位置的数，求两个数相等的概率。

 $n,m\le5\times10^4,c_i\le n$ 

# 解题思路

本题保证 $c_i\le n$ 。事实上，如果不满足这个条件，我们也可以对序列做离散化操作，因为数字的绝对大小与解题无关。

 $c_i$ 的值域相当有限，于是便可以考虑用桶来维护区间中不同数字的数量。

每次移动区间端点后，只需要考虑新加进来的这个数和前面的数相等的概率是多少，经过某些 $O(1)$ 的运算便可以得出新的概率。这符合莫队算法的使用条件。

进一步地，与其维护概率，不如维护满足相等条件的情况数，这样也免去了分母的麻烦。

# 注意事项

1. 一定要记得先分块！没有分块的莫队是纯暴力！很容易兴冲冲地开始写题然后没有做分块标记操作最后喜提TLE。
2. 询问比较函数里的 `id` 和块的 `bid` 一定要分清楚，比较的是块下标，否则就变成暴力了。
3. 4个 `while` 的顺序有讲究，应当先向两边扩，再往中间缩。

# AC代码

```cpp
#include<bits/stdc++.h>
#define ll long long
#define N 50010
using namespace std;

ll n,m;
ll c[N];
ll len,num,bid[N];
ll l,r,buck[N],ans;
string out[N];

struct query{
	ll id,l,r,ans;
	bool operator<(const query& b) const {
		if(bid[l] != bid[b.l]) return bid[l] < bid[b.l];
		else return r < b.r;
	}
};
query q[N];

inline ll f(ll x){
	if(x == 0) return 0;
	else return x*(x-1)/2;
}

inline ll gcd(ll a, ll b){
	if(b == 0) return a;
	else return gcd(b, a%b);
}

inline string ar(query w){
	ll a = w.ans, b = f(w.r-w.l+1);
	if(a == 0) return "0/1";
	ll g = gcd(a, b);
	a /= g, b /= g;
	ostringstream oss;
	oss << a << '/' << b;
	return oss.str();
}

inline void move(ll pos, ll delta){
	ll val = c[pos];
	ll b0 = buck[val], b1 = buck[val] + delta;
	buck[val] += delta;
	ll f0 = f(b0), f1 = f(b1);
	ans += (f1 - f0);
}

int main(){
//	freopen("data.in", "r", stdin);
//	freopen("ans.out", "w", stdout);
	cin >> n >> m;
	len = sqrt(n);
	num = n / len + 1;
	for(int i = 1; i <= n; i++){
		cin >> c[i];
		bid[i] = (i-1)/len + 1;
	}
	for(int i = 1; i <= m; i++){
		q[i].id = i;
		cin >> q[i].l >> q[i].r;
	}
	
	sort(q+1, q+1+m);
	
	l = q[1].l, r = l-1;
	for(int i = 1; i <= m; i++){
		// 这4个while的顺序是有讲究的！
		// 先向两边扩，再往中间缩
		// 防止r跳到l的左边出现负数答案
		while(l > q[i].l) move(--l, 1);
		while(r < q[i].r) move(++r, 1);
		while(l < q[i].l) move(l++, -1);
		while(r > q[i].r) move(r--, -1);
		q[i].ans = ans;
	}
	
	for(int i = 1; i <= m; i++){
		out[q[i].id] = ar(q[i]);
	}
	
	for(int i = 1; i <= m; i++){
		cout << out[i] << endl;
	}
	
	return 0;
}
```