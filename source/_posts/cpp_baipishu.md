---
title: 现代C++白皮书 笔记1
date: 2022-02-17
tag: 
- Cpp
category: Cpp
mathjax: false
---
现代C++白皮书 笔记1
<!--more-->
## C++14
* 0b11001010
* 0b1100'1010'0010
* 函数返回类型推导
```cpp
auto add(auto&x,auto&y)
{
    return x+y;
}
```
* constexpr中的局部变量
* 移动捕获 `[p =  move(ptr)](){};`
* 按类型访问元组 `x = get<int>(t);`
* 用户定义字面量 `3ms, 55us, 10i, "hello"s`
* 变量模版
```cpp
template <typename T>
T x = T(3.1);

cout << x<int> << " " << x<double> << endl; // 3 3.1
```
* 泛型lambda表达式
```cpp
auto get_size = [](auto& m){return m.size();};
```
在其他一些语言里面，有专门的特殊语法：
```
C#      x => x*x
JAVA    x -> x*x
D       (x){return x*x;}
```
Bjarne认为不使用特殊记法是对的，但认为应该引入概念。

## Concept 概念
概念——用于指定对于模版参数要求的编译期谓词
类型和值概念：`Buffer<unsigned char, 128>` 即值也可以作为概念的参数
（感觉BS就是让Alexander Stepanov和STL给带坏了才老想着整Concept）

Concept的问题：
```cpp
template <typename T>
concept Tickable = requires(T t)
{
    t.tick();
};

struct Foo
{
    void tick() { cout << "tick\n"; }
    void tock() {}
};

void Bar(Tickable auto &t)
{
    t.tock();
    t.tock1();
    t.tick();
}

int main()
{
    Foo f{};
    Bar(f);
}
```
tickable即可表达 可以tick的概念 例如原子钟，手表，金表，都可以属于这个概念
但C++的CONCEPT的一个问题就是： 我们在`Bar`里面可以使用`t.tock()`这样的语句。 这个时候你传`Foo`进去是可以编译通过的。但是传别的`Tickable`的东西就不一定能通过。
而还有一种做法是你只能用明确写出的接口。例如`Tickable`里面只写了`tick`，那就只能用`tick`而不能用`tock`. 
这种必须都写明白才能用的方式在 C++ 社区里称作 full template definition checking
C++ 的 Concept 不可能做到这样，因为：
1. 兼容性
你这么搞了，之前的代码怎么办？之前没有 concept 的代码都是无约束的，这么改直接全死。

2. 编译性能

我的天，你想想，C++ ，每个函数里面每一个表达式都检查一通，啧啧啧……
但是你不这么搞，那这个 Concept 的作用就没啥意思了——万一不小心用了什么没约束的操作错误信息不还是很难看。
要我说，这方面 Rust 做得太好了。我认为 Rust 的这个 Trait 真是把静态接口和所谓的“动态接口”完美地、无缝地结合到一块去了。真好。



