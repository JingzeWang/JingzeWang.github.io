---
layout: post
title:  "浪潮杯第十届山东省大学生程序设计竞赛解题报告"
date:   2019-5-14
desc: "苟利国家生死以,岂因祸福避趋之"
keywords: none
categories: [Acm]
tags: [ICPC]
icon: icon-html
---

[传送入口：The 10th Shandong Provincial Collegiate Programming Contest](https://cn.vjudge.net/contest/301710)

## [A - Calandar](https://cn.vjudge.net/problem/ZOJ-4113)

题目已知一个星球，一个月30天，一周5天，一年12个月

给定一个日期及其对应星期几，求下一个输入的日期是星期几

一个月30天，5的倍数。直接算日的差值即可。

``` c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
string fuck[5] = {"Monday", "Tuesday", "Wednesday", "Thursday", "Friday"};
int main() {
    int t;
    cin >> t;
    while(t--) {
        ll a, b, c, x, y, z, k;
        string str;
        cin >> a >> b >> c >> str;
        for(int i = 0; i < 5; ++i) if(fuck[i] == str) k = i;
        cin >> x >> y >> z;
        k = (z - c + 30 + k) % 5;
        cout << fuck[k] << endl;
    }
}
```

## [C - Wandering Robot](https://cn.vjudge.net/problem/ZOJ-4115)

给定机器人走方格的方式和重复的遍数，求其离原点的横坐标与纵坐标差值之和

有可能出现在第一遍，也有可能出现在最后一遍

倍加即可

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 1e5 + 10;
string str;
ll llabs(ll x) {
    return x > 0 ? x : -x;
}
int main() {
    ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);
    int t;
    cin >> t;
    while(t--) {
        ll n, k;
        cin >> n >> k >> str;
        ll x = 0, y = 0, res1 = 0;
        for(int i = 0; i < n; ++i) {
            if(str[i] == 'R') ++x;
            else if(str[i] == 'L') --x;
            else if(str[i] == 'U') ++y;
            else --y;
            if(llabs(x) + llabs(y) > res1) res1 = abs(x) + abs(y);
        }
        x *= k - 1, y *= k - 1;
        ll res2 = llabs(x) + llabs(y);
        for(int i = 0; i < n; ++i) {
            if(str[i] == 'R') ++x;
            else if(str[i] == 'L') --x;
            else if(str[i] == 'U') ++y;
            else --y;
            if(llabs(x) + llabs(y) > res2) res2 = abs(x) + abs(y);
        }
        cout << max(res1, res2) << endl;
    }
}
```

## [D - Game on a Graph](https://cn.vjudge.net/problem/ZOJ-4116)

给定一个连通图，1队2队轮流取边，如果不连通就输了，问赢家是哪一队

我寻思这题目不需要读边（输入保证连通）

```c++
#include <bits/stdc++.h>
using namespace std;
int main() {
    ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);
    int t;
    cin >> t;
    while(t--) {
        int k, n, m, temp;
        string str;
        cin >> k >> str >> n >> m;
        for(int i = 0; i < m; ++i) cin >> temp >> temp;
        int cxk = m - (n - 1);
        cout << (str[cxk % k] == '1' ? '2' : '1') << endl;
    }
}
```

## [F - Stones in the Bucket](https://cn.vjudge.net/problem/ZOJ-4118)

给定n个容器，里面分别装有石头，每次进行操作可以将一块石头从一个容器转移到另一个容器，或是将其移除

求最少的操作数，使所有容器中石头数相同

求个平均值就解了

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 1e5 + 10;
int a[N];
int main() {
    ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);
    int t;
    cin >> t;
    while(t--) {
        int n;
        cin >> n;
        ll sum = 0;
        for(int i = 0; i < n; ++i) cin >> a[i], sum += a[i];
        ll ave = sum / n, res = sum - ave * n;
        for(int i = 0; i < n; ++i) res += (ave > a[i]) * (ave - a[i]);
        cout << res << endl;
    }
}
```

## [K - Happy Equation](https://cn.vjudge.net/problem/ZOJ-4123)

 输入a，q，求满足$a^x \equiv x^a (\text{mod } 2^p)$的x的个数$(1\le x \le 2^p)$

暴力打表找规律，发现是小部分无序+大部分有序

规律直接上代码

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
ll qpow(ll x, ll y, ll mod) {
    ll res = 1;
    while(y) {
        if(y & 1) res = (res * x) % mod;
        x = (x * x) % mod;
        y >>= 1;
    }
    return res;
}
int main() {
    ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);
    int t;
    cin >> t;
    while(t--) {
        int a, p;
        cin >> a >> p;
        if(a & 1) cout << 1 << endl;
        else {
            if(p == 1) cout << 1 << endl;
            else if(p == 2) cout << 2 << endl;
            else {
                ll sum = 0;
                for(int i = 1; i < p; ++i) if(qpow(a, i, (1 << p)) == qpow(i, a, (1 << p))) ++sum;
                ll q = 1 << (p / a + (p % a > 0));
                sum += ((1 << p) - p) / q + (p % q == 0 || (1 << p) % q == 0);
                cout << sum << endl;
            }
        }
    }
}
```

## [M - Sekiro](https://cn.vjudge.net/problem/ZOJ-4125)

n÷2，向上取整，重复k次，求值

暴力+剪枝，等于1或0时退出循环即可

> C++

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
int main() {
    int t;
    cin >> t;
    while(t--) {
        ll n, k;
        cin >> n >> k;
        while(n != 0 && n != 1 && k--) n -= n / 2;
        cout << n << endl;
    }
}
```

> JAVA

```java
import java.util.*;
public class Main {
	public static void main(String[] args) {
		Scanner p = new Scanner(System.in);
		int t = p.nextInt();
		while(t > 0) {
			t--;
			long n = p.nextLong(), k = p.nextLong();
			while(n > 1 && k > 0) {
				n = n - n / 2;
				k = k - 1;
			}
			System.out.println(n);
		}
		return;
	}
}
```

