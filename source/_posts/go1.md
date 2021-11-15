---
title: Go Note 1
date: 2021-09-28
tag: 
- Go
category: Go
mathjax: false
---

本文是go笔记的第一篇
<!--more-->

## 切片
### 1
切片相对于对于数组部分的引用
nil为切片的零值，但空切片不等于nil
例如：
```go
package main

import "fmt"

func main() {
	var s1 =[]int{1,2,3}
	var s=s1[3:3]
	fmt.Println(s, len(s), cap(s))
	if s == nil {
		fmt.Println("nil!")
	}
}

```

You will get
```
[] 0 0
```
则s为空切片，len与cap均为0，但s不等于nil

### 2
```go
var s=[]int{1,2,3}
```
则s为切片，类型为`[]int`

```go
var s=[3]int{1,2,3}
```
s为数组 类型为`[3]int`

```go
var s=[4]int{1,2,3}
```
s为{1,2,3,0}
##
