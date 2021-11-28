---
title: ICS PPT Note 1
date: 2021-11-26
tag: 
- ICS
- Computer System
category: ICS
mathjax: true
---
计算系统概率PPT笔记1 复习期中
<!--more-->
## PPT 1-1
架构：
Application
Algorithm and Data Structure
Programming Language, Compiler
OS/VM
**ISA (Instruction Set Architecture)**
Microarchitecture
RTL
Digital Circuits/Analog Circuits
Electonic Devices
Physics

## PPT1-2
计算机是一个二进制的系统。通过操纵电子进行计算。高电压(高于某个特定值)视为1，低电压视为0.

信息的基本单位是Bit

MOS管：有N和P型
N型MOS管:三个极, **#1** , **#2** 和 **Gate**
当Gate为1时，连通
当Gate为0时，断开

P型MOS管恰恰相反
当Gate为0时，连通
当Gate为1时，断开

## PPT 2-1
进制：八进制，二进制，十六进制
无符号整数
有符号整数：
* n个bit，有$2^n$个不同的值

原码(signed Magnitude)：一个符号位，n-1个数值位。有+0,-0,导致空间的浪费
反码(1's Complement)：正数的反码等于原码。负数的反码等于原码的符号位不变，而数值位取反。
补码(2's Complement)：反码+1等于补码。
例如3bit

|||
|---|---|
|000|0|
|001|1|
|010|2|
|011|3|
|100|-4|
|101|-3|
|110|-2|
|111|-1|

### Summary:
每个计算机里面的内容都是一个数字，也就是二进制的0和1.
负数用补码(2's Complement)来表示
Overflows can be detected utilizing the carry bit
浮点数有特殊的表示法

## PPT2-2
a data type includes **representation** and **operations**.
对整数的操作：
１．　相加
２．　相减
３．　带符号扩展(Sign Extension)
例如：0001的8位扩展为0000 0001
而　1001的8位扩展为1111 1001
将sign bit进行扩展

判断overflow:　当输入的两个操作数的符号一致，而和的符号却不一样的时候。即产生了overflow.

小数：
定点数，浮点数

浮点数：IEEE754

S:1bit Exp:8bit Fraction:23bit

1,8,23 
S,Exp,Frac

$$
(-1)^S*(1.Frac)*2^{(Exp-127)}, 1\le Exp\le 254
$$

特殊：当指数部分全1，小数部分全0时，表示无穷。符号位为1表示负无穷，符号位为0表示正无穷。浮点正常表示的指数范围为00000001-11111110，即$2^{-126}~2^{127}$
若指数全为0，则为subnormal number,它表示的指数位仍然是$2^{-126}$，但小数从1.frac变为0.frac,从而可以表示更小的数

Some data types are supported directly by the instruction set architecture.
* For LC-3, there is only one supported data type: 16-bit 2’s complement signed integer 
* Operations: ADD, AND, NOT

## PPT 3-1
晶体管和逻辑门
MOS管：已学习
CMOS: Complementary MOS
使用N和P型的MOS管来构造逻辑门

题目1:构造一个3个输入的NOR门，使用CMOS

记号：
* 在线上面打斜线写4,表示这个线是4位宽的
* 小圈表示否定

## PPT 3-2
从组合逻辑到时序逻辑
Decoder
MUX
Full adder(input: A,B,C_in,output: S,C_out)
使用1位的full adder可以构造多位的加法

减法器：使用全加器和补码的原理构造，把输入B取反之后和A相加，加的时候设置全加器的C_in为1(补码要求) 即可
 

RS锁存器(RS Latch):
R:Reset (to be 0)
S:Set   (to be 1)

|R|S|功能|
|---|---|---|
|0|0|未定义
|0|1|设为0
|1|0|设为1
|1|1|保持不变

为了解决RS锁存器，当R,S同时取1的时候未定义的问题
设计D锁存器

|D|E|功能|
|---|---|---|
|~|0|保持不变
|~|1|跟随D改变

D触发器:俩D锁存器相连，功能：在clk的上升沿读取数据，其他时候保持不变.

表示多位数据:
A=10101
A[4:2]=101
A[1:0]=01

Memory:
k*m
k=2^n个locations
每个位置有m bit 的信息

**k**: Address Space
number of locations
(usually a power of 2)

**m**: Addressability
number of bits per location
(e.g., byte-addressable)

RAM:(Random Access Memory)
Static RAM(SRAM)
* fast, not very dense (bitcell is a latch)
Dynamic RAM(DRAM)
* slower but denser, bit storage must be periodically refreshed
* each bitcell is a capacitor (like a leaky bucket) that decays

ROM:(Read Only Memory)

