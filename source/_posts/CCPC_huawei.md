---
title: CCPC华为的一道趣题
date: 2021-08-22
tag: 
- Algorithm
- Cpp
- Math
category: Algorithm
mathjax: true
---

本文记录了CCPC华为比赛中的一道题目

<!--more-->

[TOC]

## 题面
骤风起，仓颉飘飘乎不自觉于孤岛焉。岛无人迹，唯有有理数二族尔。一族曰甲分之乙，
一族曰乙分之甲，甲、乙皆正整数。数之，则族族不竭其数。
鹦鹉谓仓颉：“日择二数，合其为平均或调和平均。造得一，吾送汝归！”
仓颉能归于九千九百九十九亿九千九百九十九万九千九百九十九日否？

仓颉被一阵风刮到了一个荒无人烟的小岛上，那里有两族有理数，$\frac a b$和$\frac b a$，（$a$,$b$ 为正整数），每族数有无穷多个。
鹦鹉告诉仓颉：“每天，你可以选两个已有的数 x,y，将它们合成为$\frac{x+y}{2}$或$\frac{2xy}{x+y}$。如果你能合成 1，我就送你回家！”
仓颉能在 999999999999 天内回家吗？

T 组数据。

##输入样例
```
3
1 1
1 2
5 3
```

输出样例
```
Yes
No
Yes
```

## 做法

先打表，因为是网络赛，可以用mma先求出一些较小值。
得到(1,3),(1,7),(1,15),(7,9),(3,13)等可以做到。
猜想：分子分母(注意一定要约分)的和为2的幂次的时候，可以做到。

先写一波代码试一下：
```c++
#include <iostream>
#include <vector>

using namespace std;


int gcd(int a, int b)
{
    if (!a)
    {
        return b;
    }
    if (a < b)
    {
        return gcd(b, a);
    }

    return gcd(a % b, b);
}

struct frac
{
    int up;
    int down;
    frac(int a, int b)
    {
        int d = gcd(a, b);
        up = a / d;
        down = b / d;
    }
};


frac simplify(frac x)
{
    int d = gcd(x.down, x.up);
    return frac(x.up / d, x.down / d);
}

void pr(frac x)
{
    cout << x.up << "/" << x.down << " ";
}

bool is_power(int x)
{
    if (x == 1 || x == 2)
    {
        return true;
    }
    while (x >= 3)
    {
        if (x % 2 == 1)
        {
            return false;
        }
        x = x / 2;
    }
    return x == 2;
}

bool judge(frac x)
{
    return is_power(x.up + x.down);
}

int main()
{
    int t;
    scanf("%d", &t);
    int n, m;
    while (t--)
    {
        scanf("%d", &n);
        scanf("%d", &m);
        frac x(n, m);
        if (judge(x))
        {
            cout << "Yes\n";
        }
        else
        {
            cout << "No\n";
        }
    }
}
```

发现是可以的。

## 证明
接下来考虑怎么证明。
首先可以得到，对于$2^k$个数字，通过不断对两两取平均的方式，可以得到它们的整体的平均值。即
$$
\frac{\sum_{k=1}^{k=2^n}x_k}{2^n}
$$
从而，若现有分子分母和为$2^k$的初始分数$\frac b a$,即满足$a+b=2^k$，则可看作手头有$2^k$个数，分别是$b$个$\frac{a}{b}$和$a$个$\frac{b}{a}$
其均值为
$$
\frac{b*\frac{a}{b}+a*\frac{b}{a}}{2^k}=1
$$
即可满足条件。
注意到，若在约分之前满足分子分母和为2的幂次，则约分后也满足。
但约分之前不满足的，约分后也有可能满足。例如$\frac{3}{9}$
即有如下结论：

对任意给的初始分数（可以未约分），只要分子分母满足和为2的幂次，则应输出Yes.

但注意
$$
未约分分数分子分母和为2的幂次\implies 对应的既约分数的分子分母和为2的幂次
$$

因此可以先约分，后判断，若满足分子分母和为2的幂次，则一定可以。

那么，对于既约分数，如果和不是2的幂次会怎么样？

设分数为$\frac{a}{b}$且既约，即$gcd(a,b)=1$
则等价于存在奇质数$p$使得$p|a+b$
假设手里有的数均为满足$p|分子+分母$
现考虑我们得到的新数
$(\frac x y+\frac a b)/2=\frac{xb+ay}{2yb}$,则考虑其分子分母的和。此次有可能产生约分，但不会约去p，这是因为$p\not| yb$,而这是因为:$p|yb\implies p|y \ \ \text{or}\ \ p|b$而$p|y且p|x+y\implies p|x$与$\frac{x}{y}$为既约分数矛盾，ab同理。

这可以知道我们不会约去p，因此考虑p是否整除既约后的分数的分子分母和，等价于考虑当前分数的分子分母和。
而$xb+ay+2yb\equiv -xa-xa+2xa \equiv 0 \mod p$则p整除。

同理对调和均值也有，p依然整除分子分母和。

则可知，若初始分数的分子分母和不为2的幂次，那么永远不能得到1. 因为$1=\frac11$其分子分母和为2，不含奇质因子p

证明完毕。