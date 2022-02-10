---
title: ICS 笔记3
date: 2021-12-19
tag: 
- ICS
- Computer System
category: ICS
mathjax: true
---
ICS笔记3
<!--more-->
## Chapter8-1 Subroutines
### ADT Abstract Data Types
Up to now, we have processed a single value
* an integer
* a floating point number, or 
* an ASCII character

The real world is **abstract data types**, or more colloquially data structures，far more complex than simple, single numbers. E.g.
* a company’s organization chart
* a list of items arranged in alphabetical order 

In this chapter, we will study three abstract data types: 
* stacks
* queues
* and character strings

Before we get to stacks, queues, and character strings, however, we introduce a new concept that will prove very useful in manipulating data structures: **subroutines, or what is also called functions**.

### Subroutine
A subroutine is a program fragment that. . .
* Resides in user space (i.e, not in OS)
* Performs a well-defined task
* Is invoked (called) multiple times by a user program 
* Returns control to the calling program when finished

Virtues:
* Reuse code without re-typing it (and debugging it!)
* Divide task into parts (or among multiple programmers)
* Use vendor-supplied library of useful routines that one software engineer writes a program that requires such fragments and another software engineer writes the fragments.
    * math library
    * square root, sine, and arctangent,etc.

Important: **The Call/Return Mechanism**
### 相关的指令
JSR,JSRR,RET

### JSR
0100 1 PCoffset11
作用：跳转，并且把之前的PC保存在R7
### JSRR
0100 0 00 BaseR 000000
作用：根据BaseR的值跳转，并且把PC的值保存在R7
### RET
1100 000 111 000000
作用：把R7的值赋给PC

### 例子：取负数
```cpp
Negate NOT R0,R0
ADD R0,R0,#1
RET 
```

```cpp
; call the subroutine to compute R4=-R3
AND R0,R0,#0 ; let R0=0
ADD R0,R0,R3 ; let R0=R3
JSR Negate
AND R4,R4,#0 ; let R4=0
ADD R4,R4,R0 
```
### 如何使用
Programmer must know
* Address: or at least a label that will be bound to its address
* Function: what it does
    * NOTE: The programmer does not need to know how the subroutine works, but what changes are visible in the machine’s state after the routine has run
* Arguments: what they are and where they are placed
* Return values: what they are and where they are placed

### 调用时的信息传递
Argument(s)
* Value passed in to a subroutine is called an argument
* This is a value needed by the subroutine to do its job
* Examples
* TwosComp: R0 is number to be negated
* OUT: R0 is character to be printed
* PUTS: R0 is address of string to be printed

传递方法：
How?
In registers (simple, fast, but limited number)
In memory (many, but awkward, expensive)
Both

寄存器和内存进行传递参数

Return Values
* A value passed out of a subroutine is called a return value
* This is the value that you called the subroutine to compute

How?
Registers, memory, or both
Single return value in register most common
通常，采用寄存器保存返回值
### caller-save and caller-save
在调用和返回的过程中势必会对寄存器产生修改，那么如何分配保存与恢复现场的责任呢？
一般有两种办法：
caller-save和callee-save
即责任在谁？
caller-save要求调用者来保存相关的寄存器的信息，并且恢复现场。
callee-save要求被调用者来存在相关的信息并且恢复现场。

一般都采用callee-save，只有对于函数返回值R7采用的是caller-save

Generally use “callee-save” strategy, except for return values
Same as trap service routines
Save anything that subroutine alters internally that shouldn’t be visible when the subroutine returns
Restore incoming arguments to original values (unless overwritten by return value)

Remember
**You MUST save R7 if you call any other subroutine or trap**
Otherwise, you won’t be able to return!

## Chapter8-2 Memory Model for Program Execution & the Stack
