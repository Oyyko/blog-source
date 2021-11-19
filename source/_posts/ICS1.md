---
title: ICS Note 1
date: 2021-11-17
tag: 
- ICS
- Computer System
category: ICS
mathjax: false
---
This is my notes for ICS. Source: ppt
计算系统概论笔记1 来自PPT
<!--more-->
## <center>NAMES</center>
MAR: Memory Address Register
MDR: Memory Data Register
(联想Lisp的 CAR,CDR)

## <center>LC-3</center>
ALU: ADD,AND,NOT

Registers: 8 registers. R0,R1,...,R7

Word Size:
* number of bits normally processed by ALU in one instruction
* also width of registers
* LC-3 is 16 bits

I/O:
* Devices for getting data into and out of computer memory
* Each device has its own interface, usually a set of registers like the memory’s MAR and MDR

LC-3:
* I/O: keyboard, console
* keyboard: data register(KBDR) and status reg (KBSR)
* console: data reg(CRTDR) and status reg(CRTSR) 
* frame buffer: memory-mapped pixels

CRT: cathode ray tube (CRT) 阴极射线管
Program that controls access to a device is usually called a **driver**.

## <center>CONTROL UNIT</center>
<center>Orchestrates execution of the program</center>


Instruction Register (IR) contains the current instruction.
Program Counter (PC) contains the **address** of the next instruction to be executed.

Control unit:
* reads an instruction from memory 
    * the instruction’s address is in the PC

* interprets the instruction, generating signals that tell the other components what to do
    * an instruction may take many machine cycles to complete

![Insturction Processing](/images/ICS1-1.png)

## Instruction
* The instruction is the fundamental unit of work.
* Specifies two things:
    * opcode: operation to be performed
    * operands: data/locations to be used for operation
* An instruction is encoded as a sequence of bits.  (Just like data!)
* Often, but not always, instructions have a fixed length, such as 16 or 32 bits.
* Control unit interprets instruction: generates sequence of control signals to carry out operation.
* Operation is either executed completely, or not at all. (原子性)

原子性： 比如：从账户A向账户B转1000元，那么必然包括2个操作：从账户A减去1000元，往账户B加上1000元。这2个操作必须要具备原子性才能保证不出现一些意外的问题。
原子性定义： 一个操作或者多个操作，要么全部执行并且不被打断，要么就都不执行。

JAVA中volatile关键字的含义之一： 使得JVM保证每次都从内存里面读取，而不会从CPU缓存中读取。
这被称为可见性：当一个线程修改了共享变量的值，其他线程会马上知道这个修改。当其他线程要读取这个变量的时候，最终会去内存中读取，而不是从缓存中读取。

有序性: 虚拟机在进行代码编译时，对于那些改变顺序之后不会对最终结果造成影响的代码，虚拟机不一定会按照我们写的代码的顺序来执行，有可能将他们重排序。实际上，对于有些代码进行重排序之后，虽然对变量的值没有造成影响，但有可能会出现线程安全问题。而**volatile**本身就包含了禁止指令重排序的语义。而synchronized关键字是由“一个变量在同一时刻只允许一条线程对其进行lock操作”这条规则明确的。

小结：
* synchronized关键字同时满足以上三种特性，但是volatile关键字不满足原子性。
* 在某些情况下，volatile的同步机制的性能确实要优于锁(使用synchronized关键字或java.util.concurrent包里面的锁)，因为volatile的总开销要比锁低。
* 我们判断使用volatile还是加锁的唯一依据就是volatile的语义能否满足使用的场景(原子性)

A computer’s instructions and their formats is known as its **Instruction Set Architecture** (ISA).

## LC3 ISA
### ADD(0001):
* opcode[15:12] Dst[11:9] Src1[8:6] ???[5:3] Src2[2:0]

### LDR(0110)
* opcode[15:12] Dst[11:9] Base[8:6] Offset[5:0]

### JMP(1100)
* opcode[15:12] ???[11:9] Base[8:6] ???[5:0]

means: load the content of 'Base' reg into PC

### BR(0000)
* opcode[15:12] nzp[11:9] PCoffset[8:0]

nzp: negative, zero, positive





## Process

### FETCH
Load next instruction (at address stored in PC) from memory into Instruction Register (IR).
* Load contents of PC into MAR.
* Send “read” signal to memory.
* Read contents of MDR, store in IR.
Then increment PC, so that it points to the next instruction in sequence.
* PC becomes PC+1.

### DECODE
First identify the opcode.
* In LC-3, this is always the first four bits of instruction.
* A 4-to-16 decoder asserts a control line corresponding to the desired opcode.

Depending on opcode, identify other operands from the remaining bits.
Example:
* for ADD, last three bits is source operand #2
* for LDR, last six bits is offset

### EVALUATE ADDRESS
For instructions that require memory access, compute address used for access.
Examples:
* add offset to base register (as in LDR)
* add offset to PC (or to part of PC)
* add offset to zero

### FETCH OPERANDS
Obtain source operands needed to perform operation.

Examples:
* read data from register file (ADD)
* load data from memory (LDR)

### EXECUTE
Perform the operation, using the source operands.

Examples:
* send operands to ALU and assert ADD signal
* do nothing (e.g., for loads and stores)

## STORE
Write results to destination. (register or memory)

Examples:
* result of ADD is placed in destination register
* result of memory load is placed in destination register
* for store instruction, data is stored to memory
    * write address to MAR, data to MDR
    * assert WRITE signal to memory

## Changing the Sequence of Instructions
In the FETCH phase, we incremented the Program Counter by 1.

What if we don’t want to always execute the instruction that follows this one?
examples: loop, if-then-else, function call
Need special instructions that change the contents of the PC.
These are called jumps and branches.

* jumps are unconditional -- they always change the PC
* branches are conditional -- they change the PC only if some condition is true (e.g., the contents of a register is zero)

## MISC
The clock is a signal that keeps the control unit moving.
Clock cycle (or machine cycle) -- rising edge to rising edge.
Clock generator circuit: based on crystal oscillator.

The control unit is a state machine.

Control unit will repeat instruction processing sequence as long as clock is running.
If not processing instructions from your application, then it is processing instructions from the Operating System (OS).
The OS is a special program that manages processor and other resources.

To stop the computer:
* AND the clock generator signal with ZERO
* when control unit stops seeing the CLOCK signal, it stops processing

## Summary
Instructions look just like data -- it’s all interpretation.
Three basic kinds of instructions:
* computational instructions (ADD, AND, …)
* data movement instructions (LD, ST, …)
* control instructions (JMP, BRnz, …)
Six basic phases of instruction processing:
* not all phases are needed by every instruction
* phases may take variable number of machine cycles

