---
title: A strange way to compute GCD
date: 2021-11-18
tag: 
- Algorithm
- C
- Cpp
category: Cpp
mathjax: false
---
A strange way to compute GCD in one line
<!--more-->
```cpp
int gcd(int x, int y)
{
    while (x ^= y ^= x ^= y %= x)
        ;
    return y;
}
```

有趣的是：这种写法在CPP17之前是ub（在同一个语句中多次改变一个变量的值，且反复使用该变量的值，为ub 因为标准没有规定求值的顺序），在CPP17中，对于求值的顺序做了进一步的规定，从而这种写法具备了可移植性。