# Lecture - Evaluating Computer Performance
Created on: 10-17-2022 21:17
___

==*5 classic components of a computer*==
**memory**
	- **control**
	- **datapath**
**input**
**output**


## PERFORMANCE EVALUATIONS
performance evaluation goals: buyer vs architect perspective

execution time = response time = wall clock time
	time between start and completion of a task

$$performance={1 \over executiontime}$$

absolute performance doesn't say much
→ need a **baseline** to measure **relative performance**
==*SPEEDUP*==: ratio of ${performance_x \over performance_y}$ or ${execution\;time_A \over execution\;time_B}$


##### CPU TIME = # of instr. $\times$ time for each instr.

### clock rate = ${1 \over clock\;cycle}$

##### time for each instr. = CPI $\times$ clock cycle

# ==*CPU TIME = # of instr $\times$ CPI $\times$ clock cycle*==


# throughput
\# of tasks per unit time
important for data centers, web servers, operating systems

### FLOPS
floating point operations per second – a measure of throughput

## ENERGY
$energy = power \times execution time$


# Amdahl's Law (generalization)
achievable speedup is bound by the amount that the improved feature is used
