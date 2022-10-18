# Computer Architecture Exam 1 Guide
Created on: 10-17-2022 20:25
___

Scope:
	Textbook:
		[[Ch1 - Computer Abstract and Tech]]
		[[Ch2 - Instructions]]
		[[Ch3 - Arithmetic for Computers]]
		[[Ch4 - The Processor]]
	Lectures:
		[[Lecture - Introduction to Computer Architecture]]
		[[Lecture - Evaluating Computer Performance]]
		[[Lecture - Instruction Set Architecture (ISA)]]
		[[Lecture - Binary Arithmetic]]
		[[Lecture - Data Path Intro]]
		[[Lecture - Pipelining Intro]] - see [[Ch4 - The Processor]]
		[[Lecture - Memory Hierarchy]]




# IDENTIFYING COMPONENTS ON A CHIP
![[Pasted image 20221017232854.png]]
![[Pasted image 20221018114109.png]]

RISC: reduced instruction set code
	less instructions = smaller chips = shorter travel times = higher clock rate

[Q] inclusiveness: ==*if an instruction is in L1, then it must also be in main memory*==

registers used for parameters in ARM: ==X0 ~ X7==

temporarily storing data in stack
```assm
ADDI SP SP #24  //make room for 3 registers
STUR X10, [SP, #16]
STUR X11, [SP, #8]
STUR X12, [SP, #0]
```

