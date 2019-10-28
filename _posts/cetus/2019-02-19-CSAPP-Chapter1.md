---
layout: post
title:  "CSAPP第一章-计算机系统漫游"
date:   2019-02-19
desc: "苟利国家生死以,岂因祸福避趋之"
keywords: none
categories: [Cetus]
tags: [CSAPP]
icon: icon-html
---

​	我错了我再也不拖更了QAQ

- ### hello程序的编写

  以ubuntu为例（服务器上运行，使用powershell）

  > 连接服务器

  ```shell
  ssh root@***.***.***.***
  ```

  > 安装g++编译器

  ```shell
  sudo apt-get install build-essential
  sudo apt-get install gdb
  ```

  > 使用$vi$编辑器

  ```shell
  vi hello.cpp
  ```

  > 编写代码

  ```c++
  #include <bits/stdc++.h>
  using namespace std;
  int main()
  {
      cout << "hello,world" << endl;
      return 0;
  }
  ```

  > 保存cpp文件

  按$esc$键退出编辑模式，输入$:wq$保存

  > 编译程序

  ```shell
  g++ -o hello hello.cpp
  ```

  > 运行

  ```shell
  ./hello
  ```

- ### hello程序的实现原理

  > 关于编译

  cpp文件是一个文本文档，经过编译系统G++的转化，被编译成可执行的二进制文件储存在磁盘上，输入$./hello$即可执行程序

  > 关于程序执行

  操作系统的$shell$程序通过$I/O$设备将命令读取至寄存器并存储在$RAM$中，并将可执行文件从磁盘读取至$RAM$

  接着调用main函数，开始执行程序代码，程序使用运行堆栈（stack，堆和栈）储存局部变量和返回地址。main函数中执行将$"hello,world\backslash n"$字符串复制到$CPU$中的寄存器，转换为模拟信号打印在屏幕上。

- ### Amdahl定律

  > 公式

  $T_{old}$：系统执行某应用程序所需时间

  $\alpha$：系统某部分所需执行时间与$T_{old}$比值

  $k$：该部分性能提升比例

  $T_{new}$：总执行时间

  $S$：加速比

  $S_{\infty}$：$\lim_{k\to\infty}S$

  $$
  T_{new}
    =(1-\alpha)T_{old}+\frac{(\alpha T_{old})}{k}
    =T_{old}[(1-\alpha)+\frac{\alpha}{k}]
    \tag{1}
  $$


$$
S=\frac{1}{(1-\alpha)+\frac{\alpha}{k}}
\tag{2}
$$

$$
S_{\infty}=\frac{1}{1-\alpha}\tag{3}
$$
