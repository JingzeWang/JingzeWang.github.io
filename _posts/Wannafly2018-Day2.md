---
layout: post
title:  "CCPC-Wannafly Winter Camp 2018 Day2"
date:   2019-1-21
desc: "为什么我这么菜"
keywords: none
categories: [Acm]
tags: [Wannafly Winter 2018]
icon: icon-html
---

## A Erase Numbers II

### 题解：

先找到数列中最大的数maxn，然后找到该数左边和右边最大的数maxl和maxr，分别和maxn拼接，比较大小

特别注意此题爆long long

```c++
//Time: 3ms Memory: 3MB
#include <bits/stdc++.h>
using namespace std;
#define ull unsigned long long
ull a[6005];
int main()
{
    ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);
    int t;
    cin >> t;
    for(int i = 1; i <= t; ++i)
    {
        int n, k = -1; ull maxn = 0;
        cin >> n;
        for(int j = 0; j < n; ++j)
        {
            cin >> a[j];
            if(a[j] > maxn) maxn = a[j], k = j;
        }
        ull maxl = 0, maxr = 0;
        for(int j = 0; j < k; ++j)
            if(a[j] > maxl) maxl = a[j];
        for(int j = k + 1; j < n; ++j)
            if(a[j] > maxr) maxr = a[j];
        cout << "Case #" << i << ": ";
        if(maxl == 0 && maxr == 0) 
        {
            cout << maxn << endl;
            continue;
        }
        if(maxl)
        {
            ull temp = maxn;
            while(temp) maxl *= 10, temp /= 10;
            maxl += maxn;
        }
        if(maxr)
        {
            ull temp = maxr;
            while(temp) maxn *= 10, temp /= 10;
            maxn += maxr;
        }
        cout << max(maxl, maxn) << endl;
    }
    return 0;
}
```

## B Erase Numbers I

### 题解：

最大化剩余数字，则需最小化删除数字的长度

枚举这样的两个数字，需要判断至多三段字符串拼接后的大小

~~后缀数组?~~

```
unsolved
```

## E Power of Function

### 题解：

```c++
unsolved
```

## F Quicksort

### 题解：

```

```

## H Cosmic Cleaner

### 题解：

<p>积分或者球缺体积公式</p>

$$
球体积公式：V=\frac{4}{3}\pi R^{3}
$$

$$
球缺体积公式：V=\pi H^{2}(R-\frac{H}{3})
$$

```c++
unsolved
```

## K Sticks 

### 题解：

dp

```c++

```

## L Pyramid

### 题解：

