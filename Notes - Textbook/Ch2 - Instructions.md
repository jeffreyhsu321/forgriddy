# Ch2 - Instructions
CS 3339 Computer Architecture - Week 
Created on: 08-30-2022 09:55

Links:
- [Zybook](https://learn.zybooks.com/zybook/TXSTATECS3339LehrFall2022?selectedPanel=assignments-panel)
[[ToC - CS3339 Computer Architecture]]
- [ ] status
	- [ ] lecture
	- [x] textbook
---

## 2.1 Introduction
**instruction set**: the vocabulary of commands understood by an architecture
		ARMv8: chosen full instruction set
		LEGv8: pedagogic subset
		other ex:
				MIPS (since 1980s)
				ARMv7 (32bits, not that similar to ARMv8)
				Intel x86

**stored program concept**: 
	idea that instructions and data of many types can be stored in memory as numbers and thus easy to be changed ^b61cbc

##### design principles of hardware:
1. ==*Simplicity favors regularity*==
	- ex: fixed # of operands (3)


---
## 2.2 LEGv8 Instruction Charts

^ca6e5c

name | example | comments
--|--|--
32 registers | X0 … X30, XZR | fast location for data. (LEGv8) data must be in register to perform arithmetic. XZR = zero
$2^{62}$ memory words | Memory[0], Memory[4], … , Memory[4,611,686,018,427,387,904] | accessed only by data transfer instructions. (LEGv8) uses byte addresses so sequential doubleword addresses differ by 8. memory hold data structures, arrays, and spilled registers
#### arithmetic
instruction | example | meaning | comments
--|--|--|--
add | ADD x1, x2, x3 | x1 = x2 + x3 |
subtract | SUB x1, x2, x3 | x1 = x2 - x3|
add immediate | ADDI x1, x2, #20 | x1 = x2 +20 | used to add constants
subtract immediate | SUBI x1, x2, #20 | x1 = x2 - 20 |
add and set flags | ADDS x1, x2, x3 | x1 = x2 + x3 | add, set condition codes
subtract and set flags | SUBS x1, x2, x3| x1 = x2 - x3|
#### data transfer
instruction | example | meaning | comments
--|--|--|--
load register | LDUR x1, [x2, #40] | x1 = Memory[x2 + 40] | doubleword from memory to register
store register | STUR x1, [x2, #40] | Memory[x2+40] = x1 | doubleword from register to memory
#### logical
instruction | example | meaning | comments
--|--|--|--
and | AND x1, x2, x3 | x1 = x2 & x3
inclusive or | ORR x1, x2, x3 | x1 = x2 \| x3
exclusive or | EOR x1, x2, x3 | x1 = x2 ^ x3
and immediate | ORRI x1, x2, #20 | x1 = x2 & 20 | bit by bit AND reg with constant
logical shift left | LSL x1, x2, #10 | x1 = x2 << 10
logical shift right | LSR x1, x2, #10 | x1 = x2 >> 10
#### conditional branch
instruction | example | meaning | comment
--|--|--|--
compare and branch on equal 0 | CBZ x1, #25 | if (x1\==0) goto PC+4+100 | equal 0 test; PC-relative branch
compare and branch on not equal 0 | CBNZ x1, #25 | if(x1!=0) goto PC+4+100
branch conditionally | B.cond #25 | if(condition) goto PC+4+100
#### unconditional jump
instruction | example | meaning | comment
--|--|--|--
branch | B #2500 | goto PC+4+10000 | branch to target address; PC-relative
branch to register | BR x30 | goto x30 | for switch, procedure return
branch with link | BL #2500 | x30 = PC+4; PC+4+10000 | for procedure call PC-relative

---
## 2.3 Operands of the Computer Hardware
> ==operands of arithmetic instructions are restricted in registers==

**word**: a natural unit of access in a computer, typically a group of **32 bits**
**doubleword**: a natural unit of access in a computer, typically **64 bits** (8bytes)
		==the size of a register in LEGv8==

##### design principles of hardware:
2. ==*Smaller is faster*==
	- reason for having only 32 registers as to not increase clock cycle (longer physical dist.)

NAND, NOR are easier to implement and are faster

memory is just a large, single-dimensional array:
		**address** is the index to elements in the array
data transfer instruction**: command to move data between memory and registers

LDUR
	- **base address**: the starting address of an array in memory
	- **base register**: register that holds the array's base address
	- **offset**: constant value added to base address to locate a particular array element
ex:
	given:
			X22 holds the base addr of an array in memory (addr: 5000)
	to load the 2nd element of the array (addr: 5000 + 8\*1) into register X9:
			LDUR X9, [X22, #8]

example of a LEGv8 memory (addresses of sequential doublewords differ by 8)![[Pasted image 20220830130833.png]]
==while each byte is addressable, LEGv8 convention is to access memory by doublewords (8bytes)==
		ex: an array of doublewords A has a base addr of 2000.
		then the addr of A[9] would be 2000 + 8\*9 = 2072

**alignment restrictions**: that data transfer operations are in set # of bytes

>[!caution] ADD vs ADDI

example load and store:
![[Pasted image 20220830132428.png]]

> [!important] MIPS: destination register is actually at the end of binary instruction


## 2.5 Representing Instructions in the Computer

**instruction format**: a form of representation of an instruction composed of **fields** of binary \#
	**opcode**: the field that denotes the operation and format of an insturction
	ex: LEGv8 instruction fields: ADD X1, X4, X3  (total 32 bits)
| opcode | RM | shamt | Rn | Rd |
|--|--|--|--|--|
| ADD (10001011000) | X3 (00011) | unused (000000) | X4 (00010) | X1 (00001) |
| 8 bits | 5 bits | 6 bits | 5 bits | 5 bits |

hexadecimal numbers and conversions

##### design principles of hardware:
3. ==*good design demands good compromise*==
	ex: to keep all instructions the same length, LEGv8 have different instruction formats

instruction types (LEGv8):
		- R-type (r=register): ex: add, sub
		- I-type (i=immediate): ex: addi, subi  |  11 bits form the field with immediate const val
		- D-type: ex: LDUR, STUR

**stored-program concept**: 
	1. instructions are represented as numbers
	2. programs are stored in memory to be read or written just like data
led to what computers are today; let the computing genie out of the bottle


## 2.6 Logical Operations
shifting <small>deja vuuuuu</small>
		==shifting left by i is the same as multiplying by $2^i$ ==

OR: used to **set bits**
AND: used to **mask bits**
EOR: exclusive or

ex:
	ORI X10, X10, 1: forces the rightmost bit of X10 to be 1

ex:
	LSL X10, X10, 3: multiplies X10 by 8

to isolate a field, use AND (as a mask) or LSL followed by LSR

ex:
	goal: set right-most 3 bits of X2 to be bits 5,4,3 in X1, then set othe X2 bits to 0
		LSR X1, X1, #3
		ANDI X2, X1, #7


---
## 2.7 Instructions for Decisions
**conditional branch**: instructions that tests a value and allows for subsequent transfer of control to a new addr. in program based on the outcome of the test

CBZ (compare and branch if zero) and CBNZ
if else statement:
```pseudo
if (X3 == X4)
	X5 = X5 + X5;
else
	X5 = X5 + X6;
```
in LEGv8:
```LEGv8
	SUB X10 X3 X4
	CBNZ L1
	ADD X5 X5 X5
	B L1
L1:
	ADD X5 X5 X6
```

generic loop:
```LEGv8
Loop:
	CBNZ X0, Exit
	# loop body
	B Loop
Exit:
```

while loop:
```C
while (save[i] == k) i += 1;
```
in LEGv8:
	- X25: base addr of save[]
	- X24: value of k
	- X22: value of i
```LEGv8
Loop:

//load save[i]--------------
	//multiply i by 8 due to byte addressing
	LSL X10, X22, #3    // tmp reg X10 = i*8

	//addr of save[i] = base addr of save[] + X10
	ADD X10, X10, X25   // X10 = addr of save[i]

	//load save[i] into tmp reg X9
	LDUR X9, [X10, #0]  // tmp reg X9 = save[i]

//while loop-----
	//set up loop condition
	SUB X11, X9, X24    // X11 = save[i] - k

	//conditional branch
	CBNZ X11, Exit      // go to Exit if save[i] != k (X11 != 0)

	//increment i
	ADDI X22, X22, #1   // i++

	//end of loop
	B Loop              // go to Loop

Exit:
```

**condition codes / flags**
	- 4 extra bits to record what occurred during an instruction
	- add S to end of instructions (ADD, ADDI, AND, ANDI, SUB, SUBI) to set condition code
			N: negative - the result that set the condition code had a 1 in the most sig bit
			Z: zero - the result that set the condition code was 0
			V: overflow - the result that set the condition code overflowed
			C: carry - the result that set the condition code had a carry out of the most sig bit

B.cond:
	EQ =
	NE !-
	LT <
	LE ≤
	GT >
	GE ≥

XZR as destination register when setting condition codes??

example:
implementing while (X16 < X17) { //do stuff }
```
Loop:
	SUBS XZR, X16, X17
	B.GE Exit
	//do stuff
	B Loop
Exit:
```

**out of bounds check shortcut** for 0 ≤ x < y
```
SUBS  XZR, X, Y
B.HS IndexOutOfBounds
```

case/switch statements
	**branch address table / branch table**: a table of addrs of alternative instruction sequences


---
## 2.8 Supporting Procedures in Computer Hardware

**procedures**: a stored subroutine that performs a specific task based on its parameters

LEGv8 conventions for procedures:
	**X0 - X7**: eight parameter registers to pass params or returns
	**LR (X30**): one return address register to return to point of origin
**BL: branch and link**
	instruction that branches to an addr while saving the addr of the following instr into LR
return address
	a link to the calling site that allows a procedure to return to the proper address
		`BR LR` to return to instr following the procedure call

**program counter (PC)**: register containing the addr of the instr being executed

> [!question] PC is currently at addr 1000. The program calls a procedure using BL Foo. What happens to LR?
> 
> A: BL sets LR to the addr of the next instruction: 1004

#### on register spilling
Procedures temporarily use 8 registers to pass parameters and return values. After the procedure is done, it would need to cover its track and restore the 8 registers back to their original values before the procedure call.

To temporarily keep the original values, the registers spill into memory - notably, the stack.
![[Pasted image 20220911170651.png]]

**Stack Pointer** (SP): value denoting the most recently allocated addr in a stack that shows where registers should be spilled or where old register values can be found

note: stacks grow from higher addr to lower addr (push values = subtracting from SP)

to reduce register spilling, following convention:
	- ==X9 ~ X17==: tmp registers that are *not* preserved by the callee
	- ==X19 ~ X28==: saved registers that must be preserved on a procedure call

>[!question] If a procedure will update registers X19, X20, X21, X22, the procedure should make room on the stack by ==subtracting== 32 from SP.

#### recursion and nested loops


**global pointer**: register that is reserved to point to the static area

#### review of procedures questions
Consider a procedure P that calls another procedure Q.
	- P is NOT a leaf procedure
	- P should always save LR on the stack
	- If P will write to X0 to pass a parameter to Q, P might first need to save X0 to the stack
	- The stack should be the same before and after the call to Q. (whatever Q pushes, Q pops)

#### allocating space for new data on the stack
to store varaibles that are local to the procedure but do not fit in registers 
	ex: local arrays or structs

**procedure frame / activation record**:
	the segment of the stack containing a procedure's saved registers and local variables

**frame pointer**:
	a value denoting the location of the saved registers and local variables for a given procedure
(local variables saved to the stack can be accessed as offsets from the frame pointer)

#### allocating space for new data on the heap
for static varaibles and dynamic data structures

**text segment**:
	the segment of a UNIX object file that contains the machine language code for routines in the source file

arrays of fixed length are stored in stack; dynamically allocated data structs. are typically stored on the heap


---
## 2.9 Communicating with People

American Standard Code for Information Interchange (ASCII)

// LDURB just gets one byte at (ex) 5000.
// LDUR would get the eight bytes at (ex) 5000, 5001, 5002, 5003, 5004, 5005, 5006, 5007 and combine those into a 64-bit doubleword

Java: reserves first byte to indicate string length
C: uses null terminating character


---
## 2.10 LEGv8 Addressing for Wide Immediates and Addresses

to set any 16 bits of a constant in a register:
	**MOVZ** (move wide with zeros)
		zeroes the rest of the bits of the register
			ex: MOVZ X9, 255, LSL 16
	**MOVK** (move wide with keep)
		leaves remaining bits unchanged

>[!question] What is X19 after `MOVZ X19, 7, LSL 16`?
> 00000000 00000000 00000000 00000000 00000000 00000111 00000000 00000000

##### addressing in branches
B-type: 6 bits for the operation field and 26 bits for the address field
CB-type: 6 bits operation, 5 bits operand, 19 bits address

**PC-relative addressing**:
	an addressing regime in which the address is the sum of the program counter and a constant in the instruction

#### LEGv8 addressing mode summary
**immediate addressing**
		operand is a constant within the instruction itself
**register addressing**
		operand is a register
**base addressing / displacement addressing**
		operand is at the memory location whose address is the sum of a register and a constant in the instruction
**PC-relative addressing** (LEGv8)
		the branch address is the sum of the PC and a constant in the instruction

LEGv8 conditional branches can branch $\pm2^{18}$ (pc relative) from the PC
unconditional branch (pc relative) can be $\pm 128 $ MB from the PC


---
## 2.11 Parallelism and Instructions: Syncrhonization
**data race**
	two memory accesses form a data race if they are from different threads to same location, at least one is a write, and they occur one after another

**LDXR** / load exclusive
**STXR**/ store exclusive
	instructions are used in sequence, if the contents of the memory location specified by the load exclusive are changed before the store exclusive to the same address occurs, then the store exclusive fails and does not write the value to memory
		ex: STXR X23, X9, [X20]   where X9 stores fail/success
	used when cooperating threads of a parallel program need to synchronize to get proper behavior for reading and writing shared data


---
## 2.12 Translating and Starting a Program
[see [[Ch2 -- Lecture Supplemental - Instruction Set Architecture (ISA)#^cbe0d9]]]

assembly language: a symbolic language that can be translated into binary machine lang.

pseudoinstruction
	a common variation of assembly language instructions often treated as if it were an instruction in its own right

symbol table
	a table that matches names of labels to the addresses of the memory words that instructions occupy

**linker**
	systems program that combines independently assembly machine language programs and resolves all undefined labels into an executable file

**executable file**
	a functional program in the format of an object file that contains no unresolved references
	- can contain tables and debugging information

**loader**
	a systems program that places an object program in main memory so that it is ready to execute

DLLs: library routines that are linked to a program during execution

advantages of an **interpreter** over a **translator**: machine independence