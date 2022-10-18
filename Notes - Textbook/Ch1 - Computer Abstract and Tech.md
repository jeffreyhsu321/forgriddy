# Ch1 - Computer Abstract and Tech
CS 3339 Computer Architecture - Week 1
Created on: 08-26-2022 14:16

Links:
- [Zybook](https://learn.zybooks.com/zybook/TXSTATECS3339LehrFall2022?selectedPanel=assignments-panel)
[[ToC - CS3339 Computer Architecture]]
- [ ] status
	- [ ] lecture
	- [ ] textbook
---

### 1.1 Introduction
#### classes of computers:
- personal computers
- servers
- supercomputers
- embedded computers
	- largest class, widest range of applications
- personal mobile devices (PMDs)
- warehouse scale computers (WSCs)  →  cloud computers

software as a service (SaaS) delivers software and data as a service over the Internet
	- usually via a thin program (browser)
	- ex: web service, social network, etc

in the early days, minimizing mem space = performance  (no longer the case)

#### how each computer component impact performance
component | how it impacts performance
--|--
algorithm | determines # of source-level statements and # of I/O operations executed
programming language, complier, architecture | determines # of computer instr. for each source-level statement
processor and memory | determines how fast instr. can be executed
I/O system (hardware, OS) | determines how fast I/O operations can be executed
##### more indepth impacts listed in [[Ch1 - Computer Abstract and Tech#^2dbc58]]

#### -bibytes
terabyte (TB: $2^{40}$ bytes)  vs  **tebibyte** (TiB: $10^{12}$ bytes)

___
### 1.2 Eight Great Ideas in Architecture
axioms:
	I. design for **Moore's Law**
		- integrated circuit resources double every year
	II. use **abstraction** to simplify design
		- characterize design at different levels of representation
	III. make **common case fast**
	IV. performance via **parallelism**
	V. performance via **pipelining**
	VI. performance via **prediction**
	VII. **hiearchy of memories**
	VIII. dependency via **redundancy**

___
### 1.3 Below the Program
**systems softwares**: os, compliers, loaders, assemblers **(runs on hardware)**
// vs application software

operating system:
	- interfaces between user programs and hardware
	- handles basic I/O
	- allocates storage/memory
	- **provides services and supervisory functions**

**complier**: translates from high-level to assembly
**assembler**: translates from assembly to binary
>[ex] assembly lang. (ADD A, B)  vs  machine lang. (0011100110111101)

>[!Question]
>(12 is 1100 in binary)
>If a computer's memory location contains 1100, it DOES NOT necessarily mean the location memory contains the number 12.
>
>1100 could be a set of instruction instead. 

advantages of high-level lang.:
	- natural
	- promotes productivity
	-==allows programs to be independent of any particular machine==

___
### 1.4 Under the Covers
five classic components:
	- input 
			- [[Ch5 - Large and Fast: Exploiting Memory Hierarchy]]
			- [[Ch6- Parallel Processor from Client to Cloud]]
	- output
			- [[Ch5 - Large and Fast: Exploiting Memory Hierarchy]]
			- [[Ch6- Parallel Processor from Client to Cloud]]
	- memory
			- [[Ch5 - Large and Fast: Exploiting Memory Hierarchy]]
	- (processor)
		- datapath
				- [[Ch3 - Arithmetic for Computers]] 
				- [[Ch4 - The Processor]] 
				- [[Ch6- Parallel Processor from Client to Cloud]]
		- control
				- [[Ch4 - The Processor]]
				- [[Ch6- Parallel Processor from Client to Cloud]]
>[flow] **input** data → **control** instruct **datapath** to operate on data → **output** data

##### display:
LCDs:
	- liquid crystal displays
	- thin layer of liquid polymers
	- not a source of light; controls transmission of light by bending(pass) and straightening(block)
	- **active matrix**: displays with tiny transistor switches at every pixel
pixel: represented by a set of bits
	// each pixel is **typically 24 bits** (8 bits for each of rgb)
image: set of pixels represented by **bitmaps** (matrix of bits)
**raster refresh buffer / frame buffer**: stores the bitmap
	// image is stored in the frame buffer → bit pattern per pixel is then read out to the graphics display at the refresh rate

#### **integrated circuits (chips)**: <small> //not neccesary just the cpu (ex: DRAM mem chip) </small>
central processor unit (CPU): contains the datapath and control
	- **datapath** (brawn): performs arithmetic operations
	- **control** (brain): commands the datapath, memory, and I/O according to instructions
memories built as chips:
	- **DRAM** (dynamic random access mem):  ram $\Leftarrow$ access time same for any data in mem
	- **SRAM** (static random access memory):  faster, less dense, more expensive than DRAM
cache (usually SRAM):
	- small, fast memory that acts as a buffer for a slower, larger memory

#### abstraction through application binary interface (ABI):
**application binary interface (ABI)**
	- **instruction set architecture**:  interface between hardware and lowest-level hardware
		- includes instr, registers, memory access, I/O, etc
	- operating system:  encapsulates deets of I/O, allocating mem, etc low-level system funcs
architecture vs **implementation** (hardware that obeys the architecture abstraction):
	\\\\ISA allows discussions on funcs. independently from the hardware that performs them

==ISA enables implementations of varying cost and performance to run identical software==
$\Rightarrow$ [Q] ISA enables machine lang. programs to run on different hardware implementations

#### memory
**volatile / primary memory**: storage that retains data only if receiving power (ex: DRAM)
**nonvolatile / secondary memory**: storage that retains data even in absence of power
	types:
		- magnetic / hard disks: rotating mechnical devices
		- flash: cheaper, slower, more rugged than DRAM, but have finite writes

##### network
advantages:
	- communication
	- resource sharing: share I/O devices
	- remote access
**LAN** (local area network): 
	- network designed to carry data within a geographically confined area (building)
	// typically through Ethernet (1km)
**WAN** (wide area network):
	- network extended over hundreds of kms (continent)



___
### 1.5 Technologies for Building Processors and Memory
// VLSI circuit (very large-scaled integrated circuit)

##### manufacture of a chip:
silicon: semiconductor
	tiny portions of its surface can be chemically converted into..
		- excellent conductor (using microscopic copper or aluminum wire)
		- excellent insulator
		- switch areas that can either conduct or insulate under certain circumstances
process:
	ingots/rods → wafers → die (small rectangular sections cut from wafers) → packaged dies

costs: higher volumes = higher yield

---
<small>continued on 08-28-2022</small>

### 1.6 Performance
performance metrics depends on user/designer and purpose

**execution time**: (response time) total time to complete a task
**throughput**: (bandwidth) # of tasks completed per unit time
>[!info] decreasing exceution time often increases throughput

consider: does this affect how fast a single task is executed? → execution time+

##### performance metric 1
**inverse of execution time**: $${1\over execution\_time}$$
##### example - relative performance:
given $execution\_time_a = 10$, $execution\_time_b = 15$
${performance_a \over performance_b} = {execution\_time_a \over execution\_time_b} = n$
$n = {15 \over 10} = 1.5$ $\Rightarrow$ A is 1.5 as fast as B

// "improve performance" rather than "increase performance" to avoid potential confusion

#### measuring performance
time
	==elapsed time / response time / wall clock time==: total time to complete a task
		- incld. waiting for other tasks etc
		- response time experienced by the user
	==CPU execution time / CPU time==: actual time CPU spends computing for a task
			- does not incld. waiting for other tasks
		- ==user CPU time==: CPU time spent in the program
		- ==system CPU time==: time spent in the os on the behalf of the program

to maintain distinction between performance based on elasped time vs CPU time:
	- **system performance**: elapsed time on an unloaded system
	- **CPU performance**: *user CPU time*

##### example - measuring performance:
>[!question] A task runs alone on the CPU.
>The task starts by running for **5 ms**. The task then waits for **4 ms** while the os runs instructions to access disk. The CPU then idle for **2 ms** while waiting for data from disk. Finally, the task runs another **10 ms** and completes.

answers:
	1. elapsed time = 5+4+2+10 = 21 ms
	2. CPU time = 5+4+10 = 19 ms
	3. user CPU time = 5+10 = 15 ms
	4. system performance = elapsed time = 21 ms
	5. CPU performance = user CPU time = 15 ms

#### further measurements
time:
	==clock cycle== (tick, clock tick, clock period, **clock**, **cycle** ): time for 1 processor clock period
ex: processor with a clock rate of 1 GHz: 
	- clock ticks $10^9$ times per second
	- clock period is $10^{-9}$ seconds


#### CPU performance and its factors
==CPU execution time==: time taken by cpu to execute a program
		CPU clock cycles: # of clock cycles for the program
		clock cycle time: time of one clock period

**CPU execution time = # of CPU clock cycles  x  clock cycle time**
$\Rightarrow$ $$CPU execution time = {\# of \; CPU clock cycles \over clock rate}$$
##### instruction performance:
==CPI (clock cycles per instruction)==: the average # of clock cycles per instr. for a program
**CPU clock cycles = # of instructions  x  CPI**
>[!important] number of instructions is the same for identical instruction set architectures

##### final form: ==classic CPU performance equation==
CPU time = instruction count  x  CPI  x  clock cycle time
$\Rightarrow$
>[!important] $$CPU\;time = {instruction\;count \times CPI \over clock\;rate}$$

>[!important] the only complete and reliable measure of computer performance is **time**

time = seconds / program = $instructions \over program$x$clock\;cycles\over instruction$x${seconds \over clock\;cycle}$
variables (review):
	- CPU execution time (**CPU time**)
		- instruction count (determined by ISA)
		- clock cycles per instruction (**CPI**)
		- clock cycle time (**clock**, cycle, tick)
				- or clock rate (hertz, 1/clock )

instruction mix: a measure of the dynamic frequency of instructions across programs
		CPI varies by application, even among implementaions of the same ISA
		- hard to determine

#### component's impact on CPU performance
^2dbc58
component | affects | how?
--|--|--
algorithm | instr. count, CPI | Algorithm determines # of source program instructions executed $\Rightarrow$ determines # of processor instructions executed.   Algorithm may also affect CPI by favoring slower or faster instructions
programming language | instr. count, CPI | # of statements in the PL correlates to # of processor instructions. May also affect CPI due to features (ex: Java data abstraction → hidden function calls → higher CPI instructions)
compiler | instr. count, CPI | Determines how many processor instructions to translate into. Effect on CPI is varied and complex.
ISA | instr. count, CPI, clock rate | Affects the instructions needed for a function, the cost in cycles of each instruction, and over clock rate of the processor.
// minimum CPI is not always 1
		a processor with IPC (instructions per clock cycle) = 2 will have a CPI of 0.5

// clock time is usually fixed, but modern processors can vary them (ex: Intel's Turbo Mode)

---
### 1.7 The Power Wall

clock rates and power are strongly proportional

// notable in timeline: 
		- Pentium4 (++clock rate/power but performance+)
		- Prescott (thermal problems, abandon Pentium line)
		- Core 2 line reverts to simpler pipline with clock rate-- and multi-core
		- Core i5 continues multicore processor trends
→ clock rate stopped increasing around 2004

better measure for power: energy metric joules (instead of watts)

CMOS (complementary metal oxide semiconductor)

#### dynamic energy:
energy consumed when transistors switch states between 0 and 1
$$energy\;of\;a\;single\;transition \propto {1\over 2} capacitive\;load \times voltage^2$$
then power required per transistor:
$$\Rightarrow Power\propto capacitive\;load\times voltage^2\times frequency\;switched$$
where 
	- frequency switched is a function of the clock rate
	- **capacitance load** per transistor is a function of the fanout and the physical capacitance of the wires and transistors
			- **fanout**: the # of transistors connected to an output

problem: lowering voltage would make transistors leaky

problem: trouble further decreasing response time → switch focus to increase throughput
<small>(MOAR CORES)</small>

difficulties of rewriting for parallel programming: challenges include…
	- scheduling
	- load balancing
	- time for synchronization
	- overhead for communication between processors

---
### 1.9 Real Stuff: Benchmarking the Intel Core i7

workloads and benchmarks

**SPEC** (system performance evaluation cooperative)
	standard sets of benchmarks for modern computer systems
	- **SPECratio**: single measure summarizing all 12 integer benchmarks (> is better)

---
### 1.10 Fallacies and Pitfalls

pitfall: expecting the improvement of one computer aspect to increase overall performance by an amount proportional to the size of the improvement
		ex: doubling the performance of an add operation to a calculator yields a far smaller overall performance increase

**Amdahl's Law**: the performance enhancement possible with a given improvement is limited by the amount of imrpoved feature is used. (diminishing returns)
		$execution\;time\;after\;improvement = \frac{execution\;time\;affected\;by\;improvement}{amount\;of\;improvement} + execution\;time\;unaffected$

fallacy: ~~computers at low utilization use little power~~
		google's warehouse scale computers use 33% of peak power at 10% utilization

fallacy: ~~designing for performance and designing for energy efficiency are unrelated goals

**MIPS** (million instructions per second) = instruction count / ( execution time x $10^6$)
		= clock rate / ( CPI x $10^6$ )

---
###### end






