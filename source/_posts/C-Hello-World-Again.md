---
title: C Hello World Again
date: 2021-01-02
tag: 
- C
category: C
mathjax: false
---

本文是我给地空学院的学生的C语言讲座的讲稿

<!--more-->

#  Hello World Again

[TOC]

##  指针与数组

`a[i]=*(a+i)=i[a]`

`int (*x)[10]`与`int *x[10]`的区别
前者是指向一个长度为10的整型数组的**指针**
后者是一个长度为10的(指向整型变量的指针)的**数组**

指针与数组的互换
```c
for(int i=0;i<10;++i)
{
    a[i]=0;
}
  
for (int *p = a; p != (a + 10); ++p)
{
    printf("%d\n", *p);
}
  
```
##  C风格的字符串处理

###  字符串的表示

```c
char s[100]="Hello";
```
```c
char *s="Hello";
// 这是一个特殊的行为
// 注意到int *x=2是完全错误的
// 但这个语句是对的
// 编译器会开辟一块内存来存放"Hello"并用s指向它
// 但注意：这个"Hello"是不可更改的
// 例如下面的语句会出现错误
s[1]='a';
```

```c
printf("%c\n", "abc"[2]);//输出c
printf("%s\n", "abc"+1); //输出bc
//此处的"abc"会被解释为指向char数组{'a','b','c','\0'}的指针
```

```c
char s[100]={'a','b','c'};
```
```c
char *s = (char *)malloc(sizeof(char) * 10);
*s++ = 'a';
*s++ = 'b';
*s++ = 'c';
printf("%s", s - 2); //输出bc
printf("%s", s - 1); //输出c
free(s - 3); //释放掉用malloc申请的内存
  
```
###  字符串拷贝

```c
void strcpy1(char *a, char *b) //把b赋给a
{
    while (*b)
    {
        *a++ = *b++;
    }
    while (*a)
    {
        *a++ = '\0';
    }
}
  
int main()
{
    char a[100];
    char b[100];
    while (1)
    {
        scanf("%s", b);
        strcpy1(a, b);
        printf("%s\n", a);
    }
}
```

##  求值顺序

```c
printf("%c%c%c\n",getchar(),getchar(),getchar());
```
该语句在gcc编译器下的作用是：读入三个字符并倒序输出。
因为gcc编译器的实现方法是从右向左求值

##  变量作用域


```c
{
    int x=2;
}
printf("%d",x); //不会输出
```

```c
int x=8;
{
    int x=3;
    printf("%d",x);// 输出3
}
```

  

##  用宏实现max函数


`#define  MAX(x,y)  x > y ? x : y`
但考虑`MAX(1!=2,3)为1 != 2 > 3 ? 1 != 2 : 3`
由于`!=`的优先级小于`>`
因此上式为`1 != (2>3) ? 1!=2 : 3`
为`(1 != 0) ? 1!=2 : 3`
为`1 ? 1 : 3`
为`1`
而该式应该是MAX(1,3)=3
因此加括号为
`#define  MAX(x,y)  (x) > (y) ? (x) : (y)`
但考虑`3+ MAX(1,2)`
为`3 + 1 > 2 ? 1 : 2`
为`4 > 2 ? 1 : 2`
为`1 ? 1 : 2`
为`1`
而实际上应该是5
因此我们继续修改这个宏
`#define MAX(x,y) ((x) > (y) ? (x) : (y))`
但考虑`MAX(i++,j++)`
展开后为`i++ > j++ ? i++ : j++`
这会使得i与j都自增两次
为此考虑这么定义宏
```c
#define MAX(x,y)({     \
    int _x = x;        \
    int _y = y;        \
    _x > _y ? _x : _y; \
})
```
该宏会重新定义两个变量_x与_y来进行比较，从而使得MAX(i++,j++)符合要求。

  

##  函数与数组的相似点与共同点

相似点：
数组声明：int x[10]
函数声明：int sum(int,int)
数组类型：int [10]
函数类型：int (int,int)
数组指针：int (*x)[10]
函数指针：int (*sum)(int,int)
数组指针类型：int (*)[10]
函数指针类型：int (*)(int,int)

数组、函数共同点：
1.数组、函数都不可拷贝。

2.因为第1点，数组、函数不可以做函数的返回值，但函数可以返回数组的指针或函数的指针。

3.数组、函数可用于函数形参，但因为第1点，编译器会对其做处理。如果形参类型为数组，实际形参类型会转换成元素类型的指针，例如voidfun(int arr[5])等价于void fun(int arr*)。如果形参类型为函数，实际形参类型会转换成对应的函数指针类型，例如void fun (int test())等价于voidfun( int (*test)())

##  如何返回一个数组

用结构体包装一下
```c
#include <stdio.h>
  
#define MAXNUM 1000
  
struct Array
{
    int a[MAXNUM];
};
  
struct Array DoubleIt(struct Array x)
{
    struct Array y;
    for (int i = 0; i < MAXNUM; ++i)
    {
        y.a[i] = (x.a[i] << 1); //移位运算符的优先级非常低，应该在可能的情况下加上括号
    }
    return y;
}
  
int main()
{
    struct Array a = {1, 2, 3, 4, 5};
    struct Array b = DoubleIt(a);
    for (int i = 0; i < 5; ++i)
    {
        printf("%d\n", b.a[i]);
    }
}
  
```

## 推荐阅读书目：
《C专家编程》
《C陷阱与缺陷》
《征服C指针》
以上三本在图书馆应该都能借到，特别推荐《征服C指针》，是日本最受欢迎的C语言书籍之一，写的很好

# C学习经验

1. 多写一些简单的程序做实验，验证自己的想法。从实践中学习。
2. 多写代码，多写代码，多写代码。
3. 找一些好书看，不要看谭浩强啥的。。 比如《C与指针》，《C专家编程》，《C陷阱与缺陷》，《明解C指针》。
4. 学会使用搜索引擎。
5. 不要害怕写代码，其实真的不难。。这个属于技术活，写的越多越熟练。
6. C其实是比较偏向底层的语言，如果有对于计算机硬件底层相关的知识可能会更好理解。
