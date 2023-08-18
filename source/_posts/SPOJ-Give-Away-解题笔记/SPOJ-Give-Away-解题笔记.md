---
title: SPOJ Give Away 解题笔记
date: 2023-07-30 22:43:14
tags: ["算法数据结构", "题解", "数据结构", "分块", "排序"]
---

简直和 {% post_link 'UVA-12003-Array-Transformer-解题笔记'%} 一模一样。

把这题写一遍单纯是为了巩固分块实现能力。

实际写起来还真的 WA 了一发，确实有很多易错的地方，绝知此事要躬行。

尤其是循环的对象问题。是循环原序列还是块序列？想清楚再写。

```cpp
#include<bits/stdc++.h>
#define N 500100
#define Q 100100
using namespace std;

int n,x[N],q;
int op,a,b,c;

int id[N],len,num;
vector<int> block[N];

inline void change(int a, int b){
	int aid = id[a];
	x[a] = b;
	block[aid].clear();
	for(int i = (aid-1)*len+1; id[i] == aid; i++){
		block[aid].push_back(x[i]);
	}
	sort(block[aid].begin(), block[aid].end());
	return;
}

inline int query(int a, int b, int c){
	int aid = id[a], bid = id[b];
	int ans = 0;
	if(aid == bid){
		for(int i = a; i <= b; i++){ // 注意循环的范围是 [a,b] ，而不是 [aid, bid] 
			if(x[i] >= c){
				ans++;
			}
		}	
		return ans;
	}
	
	for(int i = a; id[i] == aid; i++){ // 注意循环是从 a 开始，不是从 aid 开始
		if(x[i] >= c) ans++;
	}
	for(int i = b; id[i] == bid; i--){ // 注意循环是从 b 开始，不是从 bid 开始
		if(x[i] >= c) ans++;
	}
	for(int i = aid+1; i <= bid-1; i++){
		ans += block[i].end() - lower_bound(block[i].begin(), block[i].end(), c); // 想清楚为什么是这样
	}
	return ans;
}

int main(){
	cin >> n;
	len = sqrt(n);
	num = n / len;
	
	for(int i = 1; i <= n; i++){
		cin >> x[i];
		id[i] = (i-1) / len + 1;
		block[id[i]].push_back(x[i]);
	}
	
	for(int i = 1; i <= num; i++){
		sort(block[i].begin(), block[i].end());
	}
	
	cin >> q;
	
	while(q--){
		cin >> op;
		if(op == 0){
			cin >> a >> b >> c;
			cout << query(a, b, c) << endl;
		}
		else{
			cin >> a >> b;
			change(a, b);
		}
	}
	
	return 0;
}
```