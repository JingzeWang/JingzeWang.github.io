---
layout: post
title:  "CCPC-Wannafly Winter Camp 2018 Day4"
date:   2019-1-23
desc: "为什么我这么菜"
keywords: none
categories: [Acm]
tags: [Wannafly Winter 2018]
icon: icon-html
---

## A 夺宝奇兵

### 题解：

贪心

```c++
//Time: 53ms Memory: 3MB
#include <bits/stdc++.h>
using namespace std;
int main()
{
	ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);
	long long a, b, c, d, e, f, g, h, n, m, sum = 0;
	cin >> n >> m >> e >> f >> g >> h;
	for(int i = 1; i < n; ++i)
	{
		cin >> a >> b >> c >> d;
		long long d1 = abs(a - e) + abs(b - f) + abs(c - g) + abs(d - h);
		long long d2 = abs(a - g) + abs(b - h) + abs(c - e) + abs(d - f);
		e = a, f = b, g = c, h = d;
		sum += min(d1, d2);
	}
	cout << sum + (abs(e - g) + abs(f - h));
}
```

## B 象象象棋

### 题解：

大模拟，std > 500 lines

## C 最小边覆盖

### 题解：

~~树上dfs~~方法复杂不推荐

最小覆盖边：把一个孤立的点变成非孤立的点；如果一条边连接两个度为2的点，那么这条边是多余的

## D 欧拉回路

### 题解：

分两类情况，m==2\|\|n==2

其他，

## E 黄金矿工

### 题解：

2的15次方是否拿

## F 小小马

### 题解：

1.马走一步一定从黑走到白或者从白走到黑

2.棋盘大于等于3*4的网格一定是两两联通

可以从这里直接搜索，或者分类讨论

```c++
//Time: 1ms Memory: 3MB
#include <bits/stdc++.h>
#define ll long long
using namespace std;
const int N = 100005;
int main()
{
	ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);
	int n, m;
	int sx, sy, ex, ey;
	cin>> n >> m;
	cin>> sx >> sy >> ex >> ey;
	if(n == 3 && m == 3 && ex == 2 && ey == 2)
    {
			cout << "No" << endl;
			return 0;
	}
	if(n == 2 && m == 2)
    {
		cout << "No" << endl;
		return 0;
	}
	if(m == 2)
    {
		if(sy == ey) cout << "No" << endl;
		else
        {
			if(abs(sx - ex) % 4 == 2) cout << "Yes" << endl;
			else cout << "No" << endl;
		}
		return 0;
	}
	if(n == 2)
    {
		if(sx == ex) cout << "No" << endl;
		else
        {
			if(abs(sy - ey) % 4 == 2) cout << "Yes" << endl;
			else cout << "No" << endl;
		}
		return 0;
	}
	int d = abs(sx - ex) + abs(sy - ey);
	if(d % 2 == 0) cout << "No" << endl;
	else cout << "Yes" << endl;
}
```



## G 置置置换

### 题解：

~~oeis~~

f\[i]\[j]表示前i个位置，在剩下的第j小的方案数，枚举所有大于它的k

用前缀和或后缀和维护

## H 命命命运

### 题解：

~~概率dp~~听不懂

## I 咆咆咆哮

### 题解：

Div2：先假设所有卡牌变为生物，计算收益；再枚举生物变为加场攻的收益

Div1：n <= 100000，三分，收益函数为凸性函数

```c++
//div2AC代码
//Time: 6ms Memory: 3MB
#include <bits/stdc++.h>
#define ll long long
using namespace std;
const int N = 1005;
struct node
{
	int a,b;
}s[N];
int v[N] = {0};
int main()
{
	ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);
	int n;
	cin >> n;
	for(int i = 1; i <= n; i++) cin >> s[i].a >> s[i].b;
	ll ans = 0;
	for(int i = 1; i <= n; i++) ans += (ll)s[i].a;
	int pre = 0;
	for(int i = 1; i <= n; i++)
	{
		int maxx = -1e8, p;
		for(int j = 1; j <= n; j++)
		{
			if(v[j]) continue;
			int h = s[j].b * (n - i) - s[j].a - pre;
			if(h > maxx)
            {
				maxx = h;
				p = j;
			}
		}
		if(maxx <= 0) continue;
		v[p] = 1;
		ans += maxx;
		pre += s[p].b;
	}
	cout << ans << endl;
}
```

## J 跑跑跑路

### 题解：

对于一个多元函数，最值在每一个变量的偏导都是0的地方

~~大佬1：梯度下降，调参数就过了~~

~~大佬2：共轭梯度下降~~

```c++

```

## K 两条路径

### 题解：

NULL

