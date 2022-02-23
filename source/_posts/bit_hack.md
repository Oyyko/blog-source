---
title: 位运算技巧
date: 2022-02-21
tag: 
- Cpp
- Algorithm
category: Cpp
mathjax: false
---
位运算技巧
<!--more-->
## lowbit
```cpp
inline int lowbit(int x)
{
    return x & -x;
}
```
`lowbit`返回最右侧的1
例如：
`lowbit(0b0011) == 0b1`
`lowbit(0b001100) == 0b100`
`lowbit(0b10101010) == 0b10`
`lowbit(0b100) == 0b100`

## gcc内置位运算函数

•int __builtin_ffs (unsigned int x)
返回x的最后一位1的是从后向前第几位，比如7368（1110011001000）返回4。
•int __builtin_clz (unsigned int x)
返回前导的0的个数。
•int __builtin_ctz (unsigned int x)
返回后面的0的个数，和__builtin_clz相对。
•int __builtin_popcount (unsigned int x)
返回二进制表示中1的个数。
•int __builtin_parity (unsigned int x)
返回x的奇偶校验位，也就是x的1的个数模2的结果。

## gosper's hack
用于生成$n$元集合所有的$k$元子集
```cpp
void GospersHack(int k, int n)
{
    int cur = (1 << k) - 1;
    int limit = (1 << n);
    while (cur < limit)
    {
        do_stuff(cur);
        // do something
        int lb = cur & -cur;
        int r = cur + lb;
        cur = ((r ^ cur) >> __builtin_ctz(lb) + 2) | r;
        // 或：cur = (((r ^ cur) >> 2) / lb) | r;
    }
}
```
## 枚举子集
```cpp
subset = mask
while subset != 0 do
    // subset 是 mask 的一个子集，可以用其进行状态转移
    ...
    // 使用按位与运算在 O(1) 的时间快速得到下一个（即更小的）mask 的子集
    subset = (subset - 1) & mask
end while
```

