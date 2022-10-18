# Instruction Set Architecture (ISA) 
[original link](http://www.cs.kent.edu/~durand/CS0/Notes/Chapter05/isa.html)

The _Instruction Set Architecture_ (ISA) is the part of the processor that is visible to the programmer or compiler writer. The ISA serves as the boundary between software and hardware. We will briefly describe the instruction sets found in many of the microprocessors used today. The ISA of a processor can be described using 5 catagories: 

**Operand Storage in the CPU**

Where are the operands kept other than in memory? 

**Number of explicit named operands**

How many operands are named in a typical instruction. 

**Operand location**

Can any ALU instruction operand be located in memory? Or must all operands be kept internaly in the CPU? 

**Operations**

What operations are provided in the ISA. 

**Type and size of operands**

What is the type and size of each operand and how is it specified?

Of all the above the most distinguishing factor is the first. 

The 3 most common types of ISAs are: 

1.  **_Stack_** - The operands are implicitly on top of the stack.
2.  **_Accumulator_** - One operand is implicitly the accumulator.
3.  _General Purpose Register (GPR)_ - All operands are explicitely mentioned, they are either registers or memory locations. 

Lets look at the assembly code of 

C = A + B;

in all 3 architectures:

Stack

Accumulator

GPR

PUSH A

LOAD A

LOAD R1,A

PUSH B

ADD B

ADD R1,B

ADD

STORE C

STORE R1,C

POP C

- 

- 

Not all processors can be neatly tagged into one of the above catagories. The i8086 has many instructions that use implicit operands although it has a general register set. The i8051 is another example, it has 4 banks of GPRs but most instructions must have the A register as one of its operands.  
What are the advantages and disadvantages of each of these approachs? 

### Stack

**Advantages:** Simple Model of expression evaluation (reverse polish). Short instructions.  
**Disadvantages:** A stack can't be randomly accessed This makes it hard to generate eficient code. The stack itself is accessed every operation and becomes a bottleneck.  

### Accumulator

**Advantages:** Short instructions.   
**Disadvantages:** The accumulator is only temporary storage so memory traffic is the highest for this approach.  

### GPR

**Advantages:** Makes code generation easy. Data can be stored for long periods in registers.  
**Disadvantages:** All operands must be named leading to longer instructions.

Earlier CPUs were of the first 2 types but in the last 15 years all CPUs made are GPR processors. The 2 major reasons are that registers are faster than memory, the more data that can be kept internaly in the CPU the faster the program wil run. The other reason is that registers are easier for a compiler to use. 

## Reduced Instruction Set Computer (RISC) 

As we mentioned before most modern CPUs are of the GPR (General Purpose Register) type. A few examples of such CPUs are the IBM 360, DEC VAX, Intel 80x86 and Motorola 68xxx. But while these CPUS were clearly better than previous stack and accumulator based CPUs they were still lacking in several areas: 

1.  Instructions were of varying length from 1 byte to 6-8 bytes. This causes problems with the pre-fetching and pipelining of instructions. 
2.  ALU (Arithmetic Logical Unit) instructions could have operands that were memory locations. Because the number of cycles it takes to access memory varies so does the whole instruction. This isn't good for compiler writers, pipelining and multiple issue. 
3.  Most ALU instructions had only 2 operands where one of the operands is also the destination. This means this operand is destroyed during the operation or it must be saved before somewhere. 

Thus in the early 80's the idea of RISC was introduced. The SPARC project was started at Berkeley and the MIPS project at Stanford. RISC stands for Reduced Instruction Set Computer. The ISA is composed of instructions that all have exactly the same size, usualy 32 bits. Thus they can be pre-fetched and pipelined succesfuly. All ALU instructions have 3 operands which are only registers. The only memory access is through explicit LOAD/STORE instructions.   
Thus C = A + B will be assembled as: 

LOAD  R1,A
LOAD  R2,B
ADD   R3,R1,R2
STORE C,R3

Although it takes 4 instructions we can reuse the values in the registers.

Why is this architecture called RISC? What is Reduced about it?  
The answer is that to make all instructions the same length the number of bits that are used for the opcode is reduced. Thus less instructions are provided. The instructions that were thrown out are the less important string and BCD (binary-coded decimal) operations. In fact, now that memory access is restricted there aren't several kinds of MOV or ADD instructions. Thus the older architecture is called CISC (Complete Instruction Set Computer). RISC architectures are also called _LOAD/STORE_ architectures. 

The number of registers in RISC is usualy 32 or more. The first RISC CPU the MIPS 2000 has 32 GPRs as opposed to 16 in the 68xxx architecture and 8 in the 80x86 architecture. The only disadvantage of RISC is its code size. Usualy more instructions are needed and there is a waste in short instructions (POP, PUSH). 

So why are there still CISC CPUs being developed? Why is Intel spending time and money to manufacture the Pentium II and the Pentium III?   
The answer is simple, backward compatibility. The IBM compatible PC is the most common computer in the world. Intel wanted a CPU that would run all the applications that are in the hands of more than 100 million users. On the other hand Motorola which builds the 68xxx series which was used in the Macintosh made the transition and together with IBM and Apple built the Power PC (PPC) a RISC CPU which is installed in the new Power Macs. As of now Intel and the PC manufacturers are making more money but with Microsoft playing in the RISC field as well (Windows NT runs on Compaq's Alpha) and with the promise of Java the future of CISC isn't clear at all.

An important lesson that can be learnt here is that superior technology is a factor in the computer industry, but so are marketing and price as well (if not more). 

## References For Further Reading

[Stack Computers: The New Wave, Philip J. Koopman, Jr,  � 1989 Philip Koopman, Jr.](http://www.ece.cmu.edu/~koopman/stack_computers/index.html)

Philip Koopman [Stack Computer](http://www.ece.cmu.edu/~koopman/stack.html) Web pages