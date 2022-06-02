---
title: C++ CONST
date: 2022-04-18
tag: 
- 面试
category: 面试
mathjax: false
---
C++ CONST
<!--more-->

`const_iterator`在STL中等价于指向`const`的指针。被指向的数值是不能被修改的。标准的做法是应该使用`const`的迭代器的地方，也就是尽可能的在没有必要修改指针所指向的内容的地方使用`const_iterator`

对于能加上const修饰的都加上const修饰，可以防止它们被无意间或者在疏忽的时候更改。

可以使用`.cbegin(),.cend()`来获取只读版本的迭代器