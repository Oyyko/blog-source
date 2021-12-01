---
title: Leetcode 1648
date: 2021-11-28
tag: 
- leetcode
category: leetcode
mathjax: true
---
Leetcode 1648 题解
<!--more-->
## 题目
你有一些球的库存 inventory ，里面包含着不同颜色的球。一个顾客想要 任意颜色 总数为 orders 的球。

这位顾客有一种特殊的方式衡量球的价值：每个球的价值是目前剩下的 同色球 的数目。比方说还剩下 6 个黄球，那么顾客买第一个黄球的时候该黄球的价值为 6 。这笔交易以后，只剩下 5 个黄球了，所以下一个黄球的价值为 5 （也就是球的价值随着顾客购买同色球是递减的）

给你整数数组 inventory ，其中 inventory[i] 表示第 i 种颜色球一开始的数目。同时给你整数 orders ，表示顾客总共想买的球数目。你可以按照 任意顺序 卖球。

请你返回卖了 orders 个球以后 最大 总价值之和。由于答案可能会很大，请你返回答案对 $10^9 + 7$ 取余数 的结果。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/sell-diminishing-valued-colored-balls
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

