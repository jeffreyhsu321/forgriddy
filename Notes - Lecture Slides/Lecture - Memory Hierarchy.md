# Lecture - memory Hierarchy
Created on: 10-18-2022 13:59
___

###### table of content
**memory wall and memory hierarchy**
		- registers, L1, L2, L3, DRAM
**principle of locality**
		- temporal vs spatial
**cache organization**
**cache performance**
		- hit rate, miss rate
**types of cache misses**
		- capacity, compulsory, conflict misses
**techniques for improving cache performance**
		- cache blocks, set associativity


### MEMORY WALL
---
memory speed is not catching up to CPU speed exponential growth

solutions:
	- build faster memory (hard)
	- build small fast memory
	- ==create a hierarchy of memory systems==

### CACHE
---
first check the cache; goto main memory if data is not found
typical memory hierarchy![[Pasted image 20221018141838.png]]
![[Pasted image 20221018142010.png]]

## INCLUSIVENESS
lower memories are ==*subsets*== of higher memories


# LOCALITY
---
### TEMPORAL LOCALITY
accessing same memory location frequent → high temporal locality
	keep most recently accessed data items closer to the processor

### SPATIAL LOCALITY
if a program accesses memory location M, it is likely it will access M + k soon
	move blocks consisting of contiguous words closer to the processor

