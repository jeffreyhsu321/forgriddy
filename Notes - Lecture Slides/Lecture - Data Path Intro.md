# Lecture - Data Path Intro
Created on: 10-18-2022 13:42
___

**datapath**: set of components that combine to execute an instruction
**control**: set of components that tell the datapath what to do

datapath +(with wires in binary)+ control = processor implementation

# DATAPATH ELEMENTS
## ALU
arithmetic logic unit
![[Pasted image 20221018134456.png|200]]
adders are ALU hardwired to only do addition

###### LOGIC GATES
AND OR XOR NOT

##### MULTIPLEXOR
chooses between wires/signals
![[Pasted image 20221018134608.png|200]]

## REGISTER FILE and REGISTERS
![[Pasted image 20221018134720.png|400]]

## MEMORY
- on chip memory: L1 cache
- separate data and instruction memory
- slowly than register but faster than main memory


# INSTRUCTION EXECUTION
---
### FETCH
fetch instruction
determine which instruction to fetch next
components:
	- memory
	- ALU
	- PC
![[Pasted image 20221018135008.png|400]]

### INSTRUCTION DECODE
determines opcode, extract register numbers, extract memory addresses
redirection of wires!
![[Pasted image 20221018135137.png|500]]

### EXECUTION: ALU
read register values
perform operation
write result to register
components:
	- register file
	- ALU
![[Pasted image 20221018135226.png|500]]

### EXECUTION: LOAD/STORE
read register operands
calculate addr using 16-bit offset (sign-extend)
LOAD: read memory and update register
STORE: write register value to memory
components:
	- register file
	- ALU
	- data memory unit
	- sign extension unit
LOAD
![[Pasted image 20221018135335.png|500]]
STORE
![[Pasted image 20221018135352.png|500]]

### EXECUTION: BRANCH
read register operands
compare operands
calculate target address (sign-extend offset, add PC+4)
components:
	- register file
	- ALU
	- sign extension unit
	- PC
![[Pasted image 20221018135451.png|500]]

### DATAPATH
![[Pasted image 20221018135551.png]]


# CONTROL
---
![[Pasted image 20221018135634.png]]

## ALU CONTROL
control signals needed for different classes of instructions