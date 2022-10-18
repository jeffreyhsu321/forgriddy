# Ch4 - The Processor
CS 3339 Computer Architecture - Week 
Created on: 10-01-2022 15:55

Links:
- [Zybook](https://learn.zybooks.com/zybook/TXSTATECS3339LehrFall2022?selectedPanel=assignments-panel)
[[ToC - CS3339 Computer Architecture]]
- [ ] status
	- [ ] lecture
	- [ ] textbook
---


## 4.2 Logic Design Conventions
---
datapath contains two types of **logic elements**
	==combinational element==
			an operational element (ex: AND gate, ALU)
					- operates on data
					- ==*outputs only depends on inputs*== (same output always given same input)
					- ==*logic will have some delay*== (hence, clock edges should be far enough apart)
	==state element==
			a memory element (ex: register, memory)
					- saved → computer loses power → restore
					- requires
						- two inputs (at least):
								- ==clock==
								- ==data value== to be written into the element
						- one output (at least)
					- ==*clock is used to determine when the state element should be written*==
					- ==*state elements can be read at any time (no need for clock)*==
					- also known as ==sequential==

clocking methodology - to make hardware predictable
	- **edge-triggered clocking**
			- a clocking scheme in which all state changes occur on a clock edge
			- makes simultaneous reading and writing possible and unambiguous

**control signal**
	- a signal used for multiplexor selection or for directing the operation of a functional unit
	- ==*state element is changed only when the write control signal is asserted and a clock edge occurs*==
	- **asserted**: signal is logically high or true
	- **deasserted**: signal is logically low or false
data signal
	- contains information that is operated on by a functional unit


## 4.3 Building a Datapath
---
**datapath element**
		a unit used to operator or hold data within a processor
			- LEGv8 includes: instruction and data memories, register file, ALU, adders

**program counter (PC)**
		register containing the addr of the next instruction to be executed

###### single cycle implementation of the LEGv8 instruction set
# ==incrementing PC==
![[Pasted image 20221001162259.png]]
instruction memory
		- read only →
				- combinational element
				- no control signal needed
program counter
		- state element
		- 64-bits register written at the end of every clock cycle →
				- no control signal needed
adder
		- combinational element
		- ALU that adds 4

[Q] Consider a rising clock edge that causes 3000 to be written into the PC.
> 
> The 3000 DOES NOT wait at the instruction memory input, instead, the read begins as soon as the new address arrives, without having to wait for a clock edge.
> 
(note: here, instruction memory acts like combinational logic)


# ==R-format instr.==
arithmetic-logical instructions
	- read two registers
	- perform an ALU operation on contents of the registers
	- write result to a register
ex: ADD, SUB, AND, ORR

**register file**
		a state element that consists of a set of registers (typically 32 general-purpose) that can be read and written by supplying a register number to be accessed

![[Pasted image 20221001163047.png]]
- read register: 5-bits indicating register number
- write register: 5-bits indicating register number of destination register

- write data:  64-bits value to be written
- read data: 64-bits value read from registers

- RegWrite: 1 indicates to write on a (rising) clock edge

- ALU operation: 4-bits indicating operation to be executed on read data


# ==store and load register instr.==
load and store instructions
	- compute a memory addr by adding offset value to base register
	- store: read from register the value to be stored
	- load: read from memory the value to be written into register
- requires sign-extending the 9-bit offset value to 64-bits
ex: LDUR, STUR

**sign-extend**
		to increase the size of a data item by replicating the high-order sign bit of the original data item in the high-order bits of the larger destination data item

![[Pasted image 20221001165951.png]]
- separate read and write controls, but only one may be asserted on any given clock


# ==branch instr.==
CBZ:
	- two operands
			- a register that is tested for zero
			- 19-bits offset to compute branch target address (relative) (requires sign-extend)

**branch target address**
		 the addr specified in a branch, which becomes the new program counter if the branch is taken (in LEVv8, branch target is given by the sum of offset value and addr of branch)

**branch taken**
		a branch where the branch condition is satisfied and PC becomes branch target
				(all unconditional branches are taken branches)
branch not taken
		a branch where the branch condition fails and the PC becomes the sequential instr addr

branch datapath operations
	1. compute branch target addr
	2. test register contents

![[Pasted image 20221001170608.png]]
- shift left 2 is used to throw away sign bits of offset field


# ==combining into one single datapath==
simplest: attempts to execute all instr in one clock cycle
		- no datapath resource can be used more than once per instr
				- any element needed more than once must be duplicated
				- need a memory for instructions and data
		- to **share a datapath element** between two different instruction classes, we may allow multiple connections to the input of an element using a **multiplexor** and **control signal** to **select among the multiple inputs**

R-type instr:
	- register file
	- ALU
![[Pasted image 20221001171152.png]]
Memory instr:
	- register file
	- ALU
	- data memory unit
	- sign extension unit
![[Pasted image 20221001171200.png]]
Branch instr:
	- register file
	- ALU
	- sign extension unit
	- shifter (concatenator in disguise)
	- adder
![[Pasted image 20221001171207.png]]
PC increment
![[Pasted image 20221001171218.png]]



# ==adding control units==

### ALU control
LEGv8 ALU defines 6 combinations of 4 control inputs
ALU control lines | function
-- | --
0000|AND
0001|OR
0010|add
0110|subtract
0111|pass input b
1100 |NOR

R-type: AND, OR, add, subtract
load/store register: add
CBZ: pass input b

to generate 4-bit ALU control line:
	- inputs:
			- opcode field of instruction
			- **ALUop**: ==*FIRST 2-bits control field that indicates operation..*==
					- (00) for load/store
					- (01) for CBZ
					- (10) to-be-determined by opcode
	- outputs:
			- 4-bit control lines that directly controls the ALU
example: ![[Pasted image 20221001172807.png]]

### main control unit

review: instruction formats
![[Pasted image 20221001212102.png]]
notice:
	- first register operand is always in bit 9:5 (for both R-type and load/store)
	- second register varies → extra multiplexor

![[Pasted image 20221001214954.png]]
ALUOp control line (2-bits)
		define what the 7 other control signals do informally before determining how to set them during instruction execution
signal|effect when deassert|effect when asserted
--|--|--
Reg2Loc|register num for Read Register 2 comes from Rm field|register num for Read register 2 comes from Rt field
RegWrite|none|register on Write Register input is written with value on Write Data input
ALUSrc|second ALU operand comes from second register file output (read data 2)|second ALU operand is the sign-extended lower 32 bits of instruction
PCSrc|PC is replaced by output of the adder that computes PC + 4|PC is replaced by output of the adder that computes branch target
MemRead|none|data memory contents designated by address input are put on Read Data output
MemWrite|none|data memory contents designated by address input are replaced by value on Write Data input
MemtoReg|value fed to register Write data input comes from ALU|value fed to register Write data input comes from data memory


==*remember, all state elements have the clock as an implicit input*==
==*clock is used in controlling writes*==

### datapath with both control units:
(single-cycle implementation of LEGv8 core instruction set)
		- easy to understand
		- too slow to be practical
![[Pasted image 20221002122755.png]]

### extension to handle unconditional branch
![[Pasted image 20221002123419.png]]
- additional OR gate (top right) to choose between branch target or sequential instruction
- one input to OR-gate: Uncondbranch control signal
- not shown: 
		sign-extend logic would recognize the unconditional branch opcode and sign-extend the lower 26 bits of the branch instruction to form a 64 bit address to be added to the PC


# issue
single-cycle implementation:
		- clock cycle must be the same length for every instruction
			→ clock cycle is determined by the longest path (probably load instr)
# __


## 4.5 Pipelining Overview
---
an implementation technique in which multiple instr. are overlapped in execution (parallelism)

classic steps in an LEGv8 instruction:
	1. fetch instr. from memory
	2. read registers and decode the instr.
	3. execute the operation or calculate an addr.
	4. access an operand in data memory (if necessary)
	5. write result into a register (if necessary)

![[Pasted image 20221002125208.png]]

designing instruction sets for pipelining (specifically LEGv8)
	- instructions are the same length
	- few instruction formats
	- memory operands only appear in load/store

##### pipeline hazards
==structural hazard==
		- when a planned instruction cannot execute in the proper clock cycle because the **hardware does not support the combination of instructions that are set to execute**
					- ex: caused if data access memory and instruction fetch memory is the same
					- ex: caused when both branch instr in stage 4 and add instr in stage 6 needs to use the same ALU
==data hazard==
		- when a planned instruction cannot execute in the proper clock cycle because **data that is needed to execute the instruction are not yet avaliable**
	- **load-use data hazard**
			- when data being loaded by load is not yet avaliable when it is needed by another instr
==control hazard== (branch hazard)
		- when …(error)… because instruction that was fetched was not the one that is needed
		- **(when the flow of instr addr is not what the pipeline expected)**
					- ex: cannot know which branch to take until condition, therefore, how to pipeline execute following instr in advance?
		- solutions: stalls, predictions

**forwarding** (bypassing)
		a method to resolve data hazards by retrieving the missing data element from internal buffers rather than waiting for it to arrive from registers or memory

graphical representation
![[Pasted image 20221002131016.png]]
	- shading indicates usage of the element by the instruction
			- left half: written
			- right half: read

**pipeline stall** (bubble)
		a stall initiated to resolve a hazard

excersises on data hazards and their solutions
![[Pasted image 20221002131932.png]]


## 4.6 Pipelined Datapath and Control
---
5 typical stages of the datapath (==*each takes up a single clock cycle*==)
	**IF**: instruction fetch  (IM: instruction memory)
	**ID**: instruction decode and register file read  (REG)
	**EX**: execution or address calculation  (ALU)
	**MEM**: data memory access  (DM: data memory)
	**WB**: write back  (REG)
with shared reigsters in between (ex: IF/ID register)

example using LDUR
		- shading on left = read
		- shading on right = write
		
#### instruction fetch
![[Pasted image 20221004162331.png]]

#### instruction decode
![[Pasted image 20221004162346.png]]

#### instruction execution
![[Pasted image 20221004162440.png]]

#### memory
![[Pasted image 20221004162513.png]]

#### write back
![[Pasted image 20221004162522.png]]

however, with STUR..
	- the write back register needs to be preserved until WB
![[Pasted image 20221004162859.png]]


#### showcasing clock cycle slices
![[Pasted image 20221004163047.png]]


#### showcasing pipelined datahpath with control signals
![[Pasted image 20221004163232.png]]
![[Pasted image 20221004163426.png]]

#### references
![[Pasted image 20221004163307.png]]
![[Pasted image 20221004163312.png]]

==*the setting of the control lines is completely determined by the opcode*==

==*control signals are generated in the cycles they are needed*==




==*to branch or not is determined in MEM*==

