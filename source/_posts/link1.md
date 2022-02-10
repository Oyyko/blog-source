---
title: 链表 笔记
date: 2021-12-28
tag: 
- Algorithm
- Linked-List
category: Algorithm
mathjax: true
---
链表笔记
<!--more-->
1. 判断是否有环
做法：龟兔赛跑，快慢指针
2. 判断两链表是否相交，若相交则找出交点。
```cpp
struct ListNode
{
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(NULL) {}
};

class Solution
{
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB)
    {
        ListNode *pA{headA}, *pB{headB};
        while (pA != pB)
        {
            pA = pA == nullptr ? headB : pA->next;
            pB = pB == nullptr ? headA : pB->next;
        }
        return pA;
    }
};
```