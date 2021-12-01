---
title: Week 269复盘
date: 2021-11-28
tag: 
- leetcode
category: leetcode
mathjax: true
---
Week 269复盘
<!--more-->
123水题 手速场
4并查集不能对每个时间点都建图，容易TLE，可以加入isolate操作之后，让所有的时间点共用一副图，即可。没有做出来
4也可以dfs建图然后做。

### 1 找出数组排序后的目标下标
```cpp
class Solution
{
public:
    vector<int> targetIndices(vector<int> &nums, int target)
    {
        vector<int> ans;
        sort(nums.begin(), nums.end());
        for (int i = 0, _ = nums.size(); i < _; ++i)
        {
            if (nums[i] == target)
                ans.push_back(i);
        }
        return ans;
    }
};
```
可改进的地方：
1. 我的做法是先排序，再遍历排序后的数组来找`target`出现的地方。排序用时$O(n\log n)$,查找用时$O(n)$
优化点1: 查到已经比target大的时候就不查了
优化点2: 不采用线性查找，采用找出`upper_bound`和`lower_bound`的方法来做
采用优化点2的代码：
```cpp

class Solution
{
public:
    vector<int> targetIndices(vector<int> &nums, int target)
    {
        sort(nums.begin(), nums.end());
        auto r = upper_bound(nums.begin(), nums.end(), target);
        --r;
        auto l = lower_bound(nums.begin(), nums.end(), target);
        int i = r - nums.begin();
        int j = l - nums.begin();
        vector<int> ans;
        for (int _ = j; _ <= i; ++_)
        {
            ans.push_back(_);
        }
        return ans;
    }
};
```
这种做法时间复杂度： 排序$O(n\log n)$,查找$O(\log n)$
瓶颈在排序上，那么有没有办法比排序更快呢?
有

注意到：若小于target的个数为a,等于的个数为b
则答案为$a,a+1,a+2,...,a+b-1$
则求出a,b即可
```cpp
class Solution
{
public:
    vector<int> targetIndices(vector<int> &nums, int target)
    {
        int a{}, b{};
        for (const auto &x : nums)
        {
            a += x < target;
            b += x == target;
        }
        vector<int> ans;
        for (int _{a}; _ < a + b; ++_)
        {
            ans.push_back(_);
        }
        return ans;
    }
};
```
时间复杂度：线性

### 2
