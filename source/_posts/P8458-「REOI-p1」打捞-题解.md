---
title: P8458 「REOI-p1」打捞 题解
date: 2023-06-28 21:47:10
tags:
---

如果将所有数列都扩展到 $k$ 的长度， $1e9$ 级别的数组肯定会时间空间双爆炸。

所以这个题要利用**周期性**。

不难发现，两个数列的**最小正周期为 ${\rm lcm}(l_i,l_j)$** ， 我们真正需要处理的是在这个最小正周期内的交叉积。

尽管如此，若 $l_i,l_j$ 都是质数，他们的最小正周期也可以很长很长。遍历最小正周期求交叉积依然无法AC。

通过分析那 $60\%$ 的数据我们可以发现，若 $l_i,lj$ 互质，那么，在一个最小正周期中， $a_i$ 中的每一个元素，都把 $a_j$ 中的每一个元素**乘了 $1$ 次，而且只乘了 $1$ 次**。那么我们可以利用分配律，得

$$\sum a_ia_j=\sum(a_i\sum a_j)=\sum a_i\sum a_j=sum_isum_j$$

可以在 $O(n)$ 的复杂度内解决。60pts到手！

**以此为启发，我们希望把结论推广到不互质的情况。**

若 $l_i,l_j$ 不互质，通过打表可以~~发现~~猜想：对于 $a_i$ 中的每一个元素， $a_j$ 中都有 $\frac{l_j}{\gcd(l_i,l_j)}$ 项与之相乘，且每一个参与相乘的项之间的间隔为 $\gcd(l_i,l_j)$。

下面给出严谨证明：
不妨设 $l_i<l_j$ ，若 $a_i$ 经过 $m$ 次复制、 $a_j$ 经过 $n$ 次复制后， $a_{i,t}$ 能与 $a_{j,k}$ 对应上，则有

$$(ml_i+t)\equiv(nl_j+k)\pmod {l_j}$$

说简单点就是：

$$(ml_i+t) \bmod l_j=k$$

我们给 $m$ 一个微小的扰动，令 $m'=m+1$ ，也就是再把 $a_i$ 向后复制一次。那么

$$
\begin{aligned}
k'&=(m'l_i+t)\bmod l_j \\
&=(ml_i+t+l_i)\bmod l_j \\
&=[(ml_i+t)\bmod l_j + l_i\bmod l_j]\bmod l_j \\
&=(k+l_i)\bmod l_j \\
\end{aligned}
$$

同理 

$$k''=(k+2l_i)\bmod l_j$$
$$k'''=(k+3l_i)\bmod l_j$$
$$......$$
$$k^{n}{'}=(k+nl_i)\bmod l_j$$

$n$ 最大可取到 $c=\frac{l_j}{\gcd(l_i,l_j)}$ 次，因为这么多次之后就回到最开始的 $k$ 了。

所有的 $k$ 都在 $l_j$ 上均匀分布，因此间隔为 $\gcd(l_i,l_j)$。

如果对于 $a_i$ 的每一项，都以 $gcd$ 为间隔遍历 $a_j$ 求和，时间复杂度为 $O(\frac{n^2l^2}{\gcd(l_i,l_j)})$ ，会超时。

应当首先把 $a_j$ 的隔 $\gcd$ 项和求出来（只需要求出前 $\gcd$ 项为首的隔 $\gcd$ 项和即可），然后在遍历 $a_i$ 时，直接把刚刚求好的和拿过来乘就可以了。

这样求和的循环外层是 $O(\gcd)$ ，内层是 $O( \frac{l_j}{\gcd(l_i,l_j)})$ ，求和总计是 $O(l)$ ，总的复杂度只有 $O(n^2l)$ ，可以AC。

AC代码：

```cpp
#include<iostream>
#include<cstdio>
#define M 998244353
using namespace std;

inline int read(){
    int x = 0, f = 1;
    char c = getchar();
    while(c<'0' || c>'9'){
        if(c == '-') f = -1;
        c = getchar();
    }
    while(c>='0' && c<='9'){
        x = (x<<3) + (x<<1) + (c-'0');
        c = getchar();
    }
    return  x * f;
}

inline int gcd(int a, int b){
    if(b == 0) return a;
    if(a < b) swap(a, b);
    return gcd(b, a % b);
}

inline int lcm(int a, int b){
    return (int)1ll * a * b / gcd(a, b);
}

int n,k,l[200],a[200][1010];
long long ans;

long long work(int x, int y){
    int c1[1010] = {}, c2[1010] = {};
    int s[1010] = {};
    if(l[x] > l[y]) swap(x, y);
    for(int i = 1; i <= l[x]; i++){
        c1[i] = a[x][i];
    }
    for(int i = 1; i <= l[y]; i++){
        c2[i] = a[y][i];
    }
    int gc = gcd(l[x], l[y]);
    int lc = lcm(l[x], l[y]);

    long long ans = 0;
    
    for(int i = 1; i <= gc; i++){
    	long long tmp = 0;
    	for(int j = i; j <= l[y]; j+=gc){
    		tmp += c2[j];
    		tmp %= M;
		}
		s[i] = tmp;
	}

    for(int i = 1; i <= l[x]; i++){ 
    	int h = 0;
    	h = i % gc;
    	if(h == 0) h = gc;
        ans += 1ll * c1[i] * s[h] % M;
        ans %= M;
    }
    ans *= k / lc;
    ans %= M;

    return ans;
}

int main(){
    n = read(), k = read();
    for(int i = 1; i <= n; i++){
        l[i] = read();
        for(int j = 1; j <= l[i]; j++){
            a[i][j] = read();
        }
    }

    ans = -1e9;
    for(int i = 1; i <= n-1; i++){
    	for(int j = i+1; j <= n; j++)
        	ans = max(ans, work(i, j));
    }
    cout<<ans;

    return 0;
}

```

