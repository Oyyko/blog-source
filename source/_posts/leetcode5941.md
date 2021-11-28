---
title: Leetcode 5941
date: 2021-11-28
tag: 
- Algorithm
category: Algorithm
mathjax: false
---
Leetcode 5941 周赛第四题
<!--more-->


```cpp

class Solution
{
public:
    vector<int> findAllPeople(int n, vector<vector<int>> &meetings, int firstPerson)
    {
        //并查集部分
        int fa[n];
        for (int i = 0; i < n; ++i)
        {
            fa[i] = i;
        }
        function<int(int)> find;
        find = [&](int x) -> int
        { return x == fa[x] ? x : (fa[x] = find(fa[x])); };
        auto merge = [&](int a, int b) -> void
        { fa[find(a)] = find(b); };
        auto is_connnect = [&](int a, int b) -> bool
        { return find(a) == find(b); };
        auto isolate = [&](int x) -> void
        { fa[x] = x; };

        //解题部分
        sort(meetings.begin(), meetings.end(), [&](auto &x, auto &y) -> bool
             { return x[2] < y[2]; }); //按照时间顺序排序meetings
        fa[firstPerson] = 0;           //将第一个人连接到0上
        for (int i = 0, _ = meetings.size(); i < _; ++i)
        {
            int j{};
            for (j = i + 1; j < _; ++j)
            {
                if (meetings[i][2] != meetings[j][2])
                    break;
            }
            for (int k = i; k < j; ++k)
            {
                merge(meetings[k][0], meetings[k][1]);
            }
            for (int k{i}; k < j; ++k)
            {
                if (!is_connnect(meetings[k][0], 0))
                {
                    isolate(meetings[k][0]);
                    isolate(meetings[k][1]);
                }
            }
            i = j - 1;
        }
        vector<int> ans;
        for (int i{}; i < n; ++i)
        {
            if (is_connnect(i, 0))
                ans.push_back(i);
        }
        return ans;
    }
};
```