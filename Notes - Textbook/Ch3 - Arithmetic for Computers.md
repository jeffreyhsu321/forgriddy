# Ch3 - Arithmetic for Computers
CS 3339 Computer Architecture - Week 
Created on: 09-16-2022 14:08

Links:
- [Zybook](https://learn.zybooks.com/zybook/TXSTATECS3339LehrFall2022?selectedPanel=assignments-panel)
[[ToC - CS3339 Computer Architecture]]
- [ ] status
	- [ ] lecture
	- [ ] textbook
---


## 3.2 Addition and Subtraction

subtraction is addition, but with **two's complement**
		two's complement for negative numbers: flip all bits, then add 1

overflow
	- adding operand with different signs cannot overflow
==when overflow occurs, only the sign bit is wrong==


---
## 3.3 Multiplication
the largest product resulting from a multiplication of n-bit multiplicand and m-bit multiplier is n+m bits long

### first multiplication algorithm
![[Pasted image 20220917000909.png]]
![[Pasted image 20220917000932.png]]
note:  for 64-bits multiplicand x 64-bits multiplier = 128-bits product
	- the multiplier register is 64-bits while the ==multiplicand register is 128-bits==
			- the latter ensures the original 64-bits of the multiplicand is retained during <<

### multiplication example
0010 x 0011
![[Pasted image 20220917120513.png]]

### refined multiplication algorithm
![[Pasted image 20220917114657.png]]
	- no more multiplier register (relocated to right half of product register)
	- multiplicand is halved to 64-bits (no longer shifts left, instead the product shifts right)


#### signed multiplication
remember original signs elsewhere and just multiply with positive numbers
		however, the refined algorithm works for signed numbers (lower doubleword will have the 64-bits product)

#### faster multiplication
provide one 64-bit adder for each bit of the multiplier
		inputs: multiplicand && multiplier bit, output of a prior adder

#### multiplication in LEGv8
MUL (multiply)
		returns integer 64-bit product
SMULH (signed multiply high), UMULH (unsigned multiply high)
		returns upper 64-bit of 128-bit product

==all multiply instructions ignore overflow==


---
## 3.4 Division
dividend $\div$ divisor = quotient + remainder

![[Pasted image 20220922102529.png]]
- divisor register, remainder register, ALU, are all 128-bits wide
- quotient register is 64-bits wide

- divisor is placed in the left half of the divisor register
- dividend is placed in the remainder

- divisor is subtracted from the remainder, then result put into remainder
- if negative → remainder is restored by adding divisor to remainder
			→ divisor does not go into dividend 
			→ quotient shifts left 1 bit and place 0 in least significant bit
- if positive → divisor is smaller/equal to dividend
			→ quotient shifts left 1 bit and places 1 in most significant bit
- divisor is shifted right 1 bit and repeat… (65 times)
![[Pasted image 20220922170341.png]]

### revised division algorithm
- only remainder is 128-bits (rest are 64-bits)
- combines quotient register with right half of remainder register
- speedup comes from reducing the size of the registers and ALU
![[Pasted image 20220922182015.png]]

### LEGv8
SDIV and UDIV

==faster division is NOT achieved through more ALUs (as with multiply), instead, prediction==


---
## 3.5 Floating Point
scientific notation

normalized
	number in floating-point notation with no leading 0s

floating point
	computer arithmetic that represents numbers in which the binary point is not fixed

fraction
	also known as the ==mantissa==

double precision
	a floating-point value represented in a 64-bits doubleword

single precision
	a floating-point value represented in a 32-bits word

exceptions / interrupts
	an unscheduled event that disrupts program execution; used here to detect overflow

IEEE 754 standard
![[Pasted image 20220922202813.png]]
//recall, the leading 1-bit is implicit (because are represented as 1.xxxx..)


### floating point addition
- significand of number with lesser exponent is shifted right until it matches that of the larger number
- add the significands
- normalize the sum
- check for over/under flows (exponent must be between [-126, 127])


guard
		the first two extra bits kept on the right during intermediate calculations of floating-poiunt numbers; used to improve rounding accuracy

rounding to make result fit floating-point format

//guard digit (first extra bit on right)) and round digit (rightmost extra bit)




