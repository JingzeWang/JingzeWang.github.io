---
layout: post
title:  "CCPC-Wannafly Winter Camp 2018 Day5"
date:   2019-1-24
desc: "为什么我这么菜"
keywords: none
categories: [Acm]
tags: [Wannafly Winter 2018]
icon: icon-html
---

## A Cactus Draw

### 题解：

div1：仙人掌，把仙人掌中的环横着放

div2：直接上代码

```c++
//Time: 2ms Memory: 3MB
#include <bits/stdc++.h>
using namespace std;
#define M 5010
int n, m, num[M], vis[M], xx[M], yy[M];
int head[M], nxt[M], tot, ver[M];
void add(int x, int y)
{
	nxt[++tot] = head[x];
	head[x] = tot;
	ver[tot] = y;
}
void dfs(int x, int dep)
{
	num[dep]++;
	xx[x] = dep, yy[x] = num[dep];
	vis[x] = 1;
	for(int i = head[x]; i; i=nxt[i])
	{
		int y = ver[i];
		if(vis[y]) continue;
		dfs(y, dep + 1);
	}
}
int main()
{
	scanf("%d%d", &n, &m);
	for(int i = 0; i < m; i++)
	{
		int u, v;
		scanf("%d%d", &u, &v);
		add(u, v);
		add(v, u);
	}
	dfs(1, 1);
	for(int i = 1; i <= n; i++)
        printf("%d %d\n", xx[i], yy[i]);
	return 0;
}
```

## B Diameter

### 题解：



## C Division

### 题解：

div2：欢乐的一批

```c++
//Time: 259ms Memory: 4MB
#include <bits/stdc++.h>
using namespace std;
#define ll long long
int main()
{
	ll n, k, temp, res = 0;
	priority_queue<ll>q;
	scanf("%lld%lld", &n, &k);
	while(n--)
	{
		scanf("%lld", &temp);
		res += temp;
		q.push(temp);
	}
	while(!q.empty() && k--)
	{
		temp = q.top();
		q.pop();
		if(temp >> 1) q.push(temp >> 1);
		res -= (temp - (temp >> 1));
	}
	printf("%lld", res);
}
```

div1：主席树，卡内存（我凭本事卡的内存，凭什么说我投机取巧）

## D Doppelblock

### 题解：

爆搜+剪枝

剪枝：

​	1.数不重复

​	2.两个X之外的和不超过线索

​	3.两个x之间能放的数字个数被线索值限制

## E Fast Kronecker Transform

### 题解：

暴力

求一个奇怪的卷积~~（题意看不懂QAQ）~~（此处卷积定义为1式）

FFT？
$$
<Empty \space Math \space Block>
$$

## F Kropki

## G Least Common Multiple

## H Nested Tree

## I Sorting

### 题解：

区间中大于x的数字相对位置不变，小于等于x的数字也不会变

把小于等于x的数字看为0，大于x的看为1

可以通过找第几个0或1找到原来的数字

1.维护区间里有几个0几个1

2.

## J Special Judge

### 题解：

没有公共端点就是不规范相交，有就判断是否是一个方向；

一个方向重合则不合法

叉积 = 0，点积 > 0