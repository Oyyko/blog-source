---
title: 三道面试题
date: 2021-12-09
tag: 
- 面试
- Algorithm
category: Algorithm
mathjax: true
---
三道面试题
<!--more-->

1. 不使用额外空间交换两个数
解1:加法减法
解2:异或
```cpp
void f(int &a, int &b)
{
    a = a + b;
    b = a - b;
    a = a - b;
}

void ff(int &a, int &b)
{
    a = a ^ b;
    b = a ^ b;
    a = a ^ b;
}
```

2. 1-1000的一个排列 丢失了三个数字 怎样$O(n)$时间，$O(1)$额外空间找到

3. 25个马 🐎！
每次可以让5个马赛跑
问最少几次能找出最快的三匹马

答案：7次可以。暂时不知道咋证明7次是最少的。
https://math.stackexchange.com/questions/1361065/why-6-races-are-not-sufficient-in-the-25-horses-5-tracks-problem 一个证明


