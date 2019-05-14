---
layout: post
title:  "Comet OJ - Contest #2"
date:   2019-04-28
desc: "苟利国家生死以,岂因祸福避趋之"
keywords: none
categories: [Acm]
tags: [Comet OJ]
icon: icon-html
---

传送门：[Comet OJ - Contest #2](https://www.cometoj.com/contest/37)

官方题解：[contest_36.pdf](http://static.eduzhixin.com/cometoj/solution/contest_36.pdf)

由于有官方题解了就不用码字了直接贴代码了嘤嘤嘤

为什么我要用JAVA？因为JAVA太难了（大嘘）

# A 因自过去而至的残响起舞

```java
//Time: 122ms Memory: 25MB
import java.util.*;
import java.math.*;
public class Main {
	public static void main(String[] args) {
		Scanner input = new Scanner(System.in);
		while(input.hasNextLine()) {
			try {
				long a = input.nextLong(), b = 0, sum = 0, i = 0;
				for(; sum <= a; ++i) {
					if(i < 2) {
						++sum;
						++b;
					}
					else {
						long temp = b / 2;
						sum += temp;
						b += temp;
					}
				}
				System.out.println(i);
			}
			catch(Exception e) {
				break;
			}
		}
	}
}
```

# B 她的想法、他的战斗

``` java
//Time: 140ms Memory: 28MB
import java.util.*;
import java.math.*;
public class Main {
	private static double max(double a, double b) {
		if(a > b) return a;
		return b;
	}
	public static void main(String[] args) {
		Scanner input = new Scanner(System.in);
		while(input.hasNextLine()) {
			try {
				Double l = input.nextDouble(), r = input.nextDouble(), L = input.nextDouble(), R = input.nextDouble();
				Double a = - 1 / (r - l), b = (L + R + 2 * l) / 2 / (r - l), c = - (L + R) / 2 * l / (r - l);
				//System.out.println(a); System.out.println(b); System.out.println(c);
				Double temp = -b / (2 * a);
				Double res;
				if(temp < l) {
					res = max(a * l * l + b * l + c, a * r * r + b * r + c);
				}
				else if(temp > r) {
					res = max(a * l * l + b * l + c, a * r * r + b * r + c);
				}
				else res = a * temp * temp + b * temp + c;
				res = max(0.0, res);
				System.out.println(String.format("%.4f", res));
			}
			catch(Exception e) {
				break;
			}
		}
	}
}
```

