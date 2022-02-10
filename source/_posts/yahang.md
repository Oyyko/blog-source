---
title: 压行技巧
date: 2021-12-16
tag: 
- Cpp
- Algorithm
category: Cpp
mathjax: false
---
压行技巧
<!--more-->

## 1
```cpp
for(init;check;update)
{

}
XXX
```
可以改成
```cpp
for(init; check || (XXX,0) ; update)
{
    
}
```

## 2

逗号的用法

```cpp
if()
{
    AAA;
    BBB;
    CCC;
    return DDD;
}
```

可以改成

```cpp
if()
{
    return AAA,BBB,CCC,DDD;
}
```

## 3

赋值语句和普通表达式的值

```cpp
A=func(B);
printf("%d",A);
```

可以改为

```cpp
printf("%d", A=func(B));
```

## 4

短路语句

```cpp
if(AAA)
	BBB
```

可以写成

```cpp
(!AAA)||BBB;
```

当AAA为假，则短路，不计算BBB

当AAA为真，计算BBB

## 5

利用for

```cpp
for(int i=1;i<=n;i++)
{
    for(int j=1;j<=n;j++)
        printf("%d",a[i][j]);
    puts("");
}
```

可以改为

```cpp
for(int i=1;i<=n;i++,puts(""))
    for(int j=1;j<=n;j++)
        printf("%d",a[i][j]);
```

## 6

综合运用例如

```cpp
int find(int x)
{
    return fa[x]==x?x:fa[x]=find(fa[x]);
}
```

并查集

## 7



