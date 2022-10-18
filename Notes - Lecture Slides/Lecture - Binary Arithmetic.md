# Lecture - Binary Arithmetic
Created on: 10-18-2022 12:20
___

## SIGNED BINARY REPRESENTATION

# TWO'S COMPLEMENT
steps:
	1. complement all the bits
	2. add 1
// does not matter if converting between positive or negative

###### example:
given $5_{10} = 0101_2$
determine $-5_{10}$ in two's complement binary

###### solution:
conversion:
$5_{10} = 0101_2$
	1. complement all bits → $1010_2$
	2. add one → $1011_2$

###### answer:
$-5_{10} = 1011_2$ in two's complement binary


## OVERFLOW
---
==*when adding operands with different signs or when subtracting operands with the same sign, overflow can never occur*==

sign bit will get flipped when overflow occurs → detection



# FLOATING POINT

##### SCIENTIFIC NOTATION
---
$(-1)^{sign}\times F \times 2^E$
	$where\;F = fractional\;part\;and\;E=exponent$

##### BINARY REPRESENTATION OF FLOATS
---
![[Pasted image 20221018133341.png|500]]
**range vs precision**
	- more F bits = more accuracy
	- more E bits = larger size represented

# IEEE FLOATING POINT REPRESENTATION
---
sign field | exponent field | fraction field
--|--|--
1 bit | 8 bits or 11 bits | 23 bits or 52 bits
interpretation: $(-1)^s \times (1 + fraction\times2^{exponent - bias})$

sign field:
	- 0 for non-negative
	- 1 for negative
fraction field:
	- ==*normalize to value 1.0 ≤ x < 20*==
				ex: $6.022\times 2^{60}$ to $1.505\times 2^{58}$
	- ==*store only bits past the decimal point*==
exponent field:
	= exponent + bias (127 or 1023, ensures exponent is unsigned)

###### ACCURATE ARITHMETIC
---
IEEE 754 specifies additional rounding control
	- extra bits of precision (guard, round, sticky)
	- choice of rounding modes