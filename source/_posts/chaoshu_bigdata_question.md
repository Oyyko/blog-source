---
title: 大数据题目
date: 2022-04-18
tag: 
- 面试
category: 面试
mathjax: false
---
大数据题目
<!--more-->

- 统计大量访问日志(分几百 M 和 几百 G 的场景);得出访问次数最多的前 K 个人 (单台机器实现)
- 10G文件，1G内存，找出最大的K个数，找出重复数



假设人由身份ID组成。问题可以抽象为：有大量的ID信息，例如3，5，3，3，7，。。。 问其中出现最多的ID 是哪一个

我们可以用hash的方式把大量信息分散到不同的小文件里面。之后再用小根堆的方式来找出每一个文件里面的最大的K个数 之后再合并到一起即可 如果有多台机器那么可以并行执行。

找出重复数字可以使用位图法 假设一个数字对应一个bit位  那么我们建立位图 对于新来的数字看它的那个位是不是1 如果不是 就设为1 如果是 说明这个数字重复出现过