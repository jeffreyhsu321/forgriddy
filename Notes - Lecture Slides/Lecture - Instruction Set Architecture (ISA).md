# Lecture - Instruction Set Architecture (ISA)
Created on: 10-18-2022 11:10
___

objectives:
	- understand differences between types of ISAs
	- be familiar with basic microarchitectural components
	- understand ISA design principles
			**- encoding**
					- fixed-length vs variable-length
			- handling constants, immediate instr, register spills
			- **memory addressing**
					- addressability / addressable space
					- 32 vs 64 bit
					- addressing modes (direct vs indirect)
			- **program memory layout**
					- program stack and heap
					- static and dynamic memory
			- endianness
			- control flow: PC-relative addressing

---
programming language
	- to make it easier to program complex hardware
	- ==create an abstraction layer over assembly language==


# INSTRUCTION SET ARCHITECTURE (ISA)
---
contract between programmer and hardware
[[Instruction Set Architecture AYNTK]]

## defines set of instructions that can execute on the processor
#### defines visible state of the system
#### defines how state changes in response to instruction execution

for a compiler writer:
	- ISA is a model of how a program will execute
for a hardware designed:
	- ISA is a formal definition of the correct way to execute a program


# ISA CLASSIFICATION
---
classfication by **computation model**
	- ==GPR (general purpose register): all operands are explicit in registers or memory locations==
	- stack: operands are implicitly on top of stack
	- accumulator: one operand is explictly the accumulator

classificatino by **design principle**
	- **RISC**: reduced instruction set code
			- also called load/store architectures
			- ==*make all instructions the same length of bits, less instructions provided*==
	- **CISC**: complex instruction set code
			- older architecture (still developed for backwards compatibility)


### REGISTERS
---
tiny memory inside the cpu
		- typically each hold one word
		- set of register in the **register file**
		- faster than main memory

special registers
	- PC: program counter (instruction pointer)
	- SP: stack pointer
	- FP: frame pointer (usually points to bottom of stack)
	- ZERO or XZR: hardcoded 0

typical register file
![[Pasted image 20221018115613.png|500]]

## PROGRAM COUNTER
hold the memory addr of a program instruction
		- gets updated after completion of an instruction
		- processor reads the value of the PC to determine which instruction will be executed next
		- (usually increment by 4)


### ALUs
---
built from logic gates (cheap)


### ISA DESIGN GOALS
---
come up with the set of instructions to support
		- define a set of instr to be implemented
		- define the semantics of each instr
![[Pasted image 20221018114302.png|500]]
where the machine state is defined with
![[Pasted image 20221018114329.png|250]]


# INSTRUCTION CLASSES
---
## ALU operations
arithmetic and logic operations

## Memory (Data Movement)
**STORE**
**LOAD**
**MOVE** (less common)

## Control Flow
control flow or branch instructions
	- conditional jumps
	- unconditional jumps
==implemented by manipulating the program counter==


##### SEMANTICS
---
language: **assembly (human-readable**) vs **machine (machine-readable)**


## INSTRUCTION ENCODING
---
specifies a binary encoding for each instruction
		- opcodes
		- operands (values, locations, and specifiers (dist vs src, etc))

variable-length instruction encoding: meh okay so what
**fixed-length instruction encoding**:
	- memory is cheap, unused bits are not a big problem
	- **essential for pipelining**


##### HANDLING CONSTANTS
---
1. use  hardwired registers (like XZR)
2. embed small constants in immediate instructions
![[Pasted image 20221018115443.png|500]]

what about large constants?
		- largest integer constant we can put in an immediate = size of instruction - other stuff


### REGISTER SPILLING
---
what if the program requires more registers than available?

solution:
	- store value in memory temporarily (register spilling)
	- performance heavy
	- compilers work hard to minimize this


# MEMORY ADDRESSING
---
think: registers can act like pointers

##### MEMORY ADDRESSABILITY
---
the range of memory addresses that can be specified in hardware (restricted by bit sizes)
	==*64-bit systems can address up to 2^64*==

### ADDRESSING MODE
---
absolute addressing is hard

**indirect / relative addressing**
	- ==access instructions and data using an offset from a common base address==
	- base address is usually: **SP, PC**

##### WORD
---
a natural unit of access in a computer
	(size of data most frequently accessed, usually size of a register)

Z
