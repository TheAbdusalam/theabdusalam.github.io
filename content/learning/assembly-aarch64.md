---
title: "AARCH64 Assembly"
description: "AARCH64 Assembly notes for reference."
date: "2024-08-04"
tags:
- AARCH64
- Notes
- Assembly
---

* The assembler is created by the manufacturers of that specific CPU architecture
  Certainly! Let's break down the concept of two-pass assemblers and the specifics of the process, step by step.
* Assemblers are programs that convert assembly language code into machine code (binary instructions that a computer's CPU can execute). Modern assemblers often use a two-pass process to ensure accurate translation and efficient use of labels and symbols.

&#x20;Pass One: Building the Symbol Table

1. **Reading the Input**: During the first pass, the assembler reads through the entire assembly source code.
    
2. **Tracking Locations**: It keeps track of the location (address) of each piece of data and each instruction. This is often managed using a location counter, which starts at the beginning of the memory space and increments as instructions and data are processed.
3. **Identifying Labels**: When it encounters a label (e.g., `Image:`), it notes the current value of the location counter and associates that value with the label.
4. **Building the Symbol Table**: The assembler builds a symbol table that maps each label to its corresponding address or numerical value. For example, if the label `Image` appears at address `0x0040`, the symbol table would record this association.

Pass Two: Generating Machine Code

1. **Reading the Input Again**: During the second pass, the assembler reads through the assembly source code once more.
2. **Converting Instructions**: It converts assembly instructions and data declarations into machine code. For example, an instruction like `MOV R1, Image` might be translated into a specific binary pattern that the CPU can execute.
3. **Using the Symbol Table**: Whenever a label or symbol is encountered in an instruction or data declaration, the assembler uses the symbol table created in the first pass to supply the correct numerical value. For instance, if `Image` corresponds to address `0x0040`, this value is used in the generated machine code.

### Labels in Assembly

* **Labels**: Labels are named markers used to indicate specific addresses in the program. They can refer to the location of instructions, data, functions, or blocks of code.
* **GNU Assembly Syntax**: In GNU assembly syntax, a label is defined by ending the label name with a colon (e.g., `Image:`). This colon is not part of the label but serves as a syntactical marker.

### Example

Consider a simplified example to illustrate this process:

**Assembly Source Code**:

```asm
START(0x0001):  MOV(0x0002) R0(0x0003), #0(0x0004)     ; Initialize R0 to 0
        MOV(0x0005) R1(0x0006), #5(0x0007)     ; Initialize R1 to 5
        LDR(0x0008) R2(0x000A), Image(0x000B)  ; Load the address of Image into R2
        ...
Image(0x000C):  .WORD(0x000D) 0x1234(0x000E)   ; Define a word of data at label Image
```

**Pass One (Building Symbol Table)**:

* `START` is at address `0x0000`
* `Image` is at address `0x000C` (assuming the instructions above occupy bytes up to address `0x000B`)

**Symbol Table**:

```asm
START -> 0x0000
Image -> 0x000C
```

**Pass Two (Generating Machine Code)**:

* `MOV R0, #0` ->  binary instruction for MOV R0, #0
* `MOV R1, #5` -> binary instruction for MOV R1, #5
* `LDR R2, Image` -> binary instruction for LDR R2, 0x000C (using the address from the symbol table)
* `.WORD 0x1234` -> binary representation of 0x1234 at address 0x000C


**Directives**
Important Directives Related to Symbols

1. **.equ or EQU**
   * **Purpose**: Define a constant value that can be used throughout the program.
   * **Syntax**: `symbol .equ value` or `symbol EQU value`
   * **Example**: `MAX_SIZE .equ 100` or `MAX_SIZE EQU 100`
   * **Usage**: The symbol `MAX_SIZE` can be used wherever the value `100` is needed. Changing the value in the definition changes it everywhere.
2. **.set**
   * **Purpose**: Similar to `.equ`, but allows the symbol to be redefined later in the code.
   * **Syntax**: `symbol .set value`
   * **Example**: `COUNTER .set 0`
   * **Usage**: The symbol `COUNTER` can be updated to a new value later in the code if needed.
3. **.define**
   * **Purpose**: Often used in macro assembly to define constants or macros.
   * **Syntax**: `symbol .define value`
   * **Example**: `PI .define 3.14`
   * **Usage**: The symbol `PI` can be used in place of the numerical value `3.14`.
4. **.def**
   * **Purpose**: Define a symbol, often used in older assembly languages.
   * **Syntax**: `symbol .def value`
   * **Example**: `BUFFER_SIZE .def 256`
   * **Usage**: The symbol `BUFFER_SIZE` is used to represent the value `256`.

There are four elements to assembly syntax: labels, directives, instructions, and comments. Directives are used mainly to define symbols, allocate storage, and control the behavior of the assembler. The most common assembler directives were introduced in this chapter, but there are many other directives available in the GNU Assembler.

Directives can also be used to declare macros. Macros are expanded by the assembler and may generate multiple statements. Careful use of macros can automate some simple tasks, allowing several lines of assembly code to be replaced with a single macro invocation.



A conceptual explanation of how most modern CPUs work

* Since there are limited amount of registers most of the program data are stored in the memory and loaded when ever the cpu needs it.

### Load, store and branch instructions



The part of the engineering/programming related to communicating with the hardware is called ISA ( instruction set architecture ) which is basically a set of instructions that 


* In the CPU we have the most important parts of the machine namely the ALU, CU and Registers
  * the AARCH64 architechecture provides registers called User Registers
    * What those are is layers of X `Width` x X `Size` bit cells (bit store), Width of normal 8's multiple and arbitrary Size
    * **General Purpose Register (GPR)**
      * each register is capable of storing 64 bits of data
      * *`W0-W30`* are 32-bit registers are the least significant bits, and are used if you don't require the whole 64bits of that register
      * `X0-X30` also called extended registers are used when you require 64bits storage
      * when you don't want to specify which or it doesn't matter using `R20` for example to call is enough
    * **The frame pointer**
      * It store the base of the current stack frame to keep track of the address of the callee and the caller of a given function/program. ussually `X20` on aarch64, can vary.
    * **PState Register**
      * This is a specialized register, it keeps track of the program state and contains any flags triggered by it so for example, if the last result was a Negative or Zero or if the last operation oVerflew. other flags are important for the OS but not for us.

        N - negative

        Z - Zero

        C - Carry

        V - oVerflow


    * **Stack Pointer**
      * The stack pointer is used to hold the address where the stack ends. This is commonly referred to as the of the stack, although on most systems the stack grows downwards and the stack pointer really refers to the address in the stack.
    * **Program Counter**
      * This counter points to the next operation that the processor will be executing next
      * The processor increments this register by 4( in 32bit architectures because a 4 bytes = 4 x 8 bits = 32 bits, thus of the next instruction) automagically every time to refer to the address of the next instruction
      * By moving an address into this register, the programmer can cause the processor to fetch the next instruction from the new address.



#### &#x20;   Register Addressing:

&#x20;   This mode uses the contents of a register as the memory address for load/store operations.

```Plain&#x20;Text
LDR X1, [X2]  ; Load 8 bytes from the address in X2 into X1
STR X1, [X2]  ; Store 8 bytes from X1 into the address in X2

```

#### &#x20;   Signed Immediate Offset:

&#x20;   This mode adds a signed immediate value to a base register to calculate the memory address.

```Plain&#x20;Text
LDR X1, [X2, #0x50]  ; Load 8 bytes from the address (X2 + 0x50) into X1
STR X1, [X2, #-0x50] ; Store 8 bytes from X1 into the address (X2 - 0x50)

```

#### &#x20;   Unsigned Immediate Scaled Offset:

&#x20;   This mode adds an unsigned immediate value, scaled based on the data size, to a base register.

```Plain&#x20;Text
LDR X1, [X2, #0x7FF8] ; Load 8 bytes from the address (X2 + (0x7FF8 << 3)) into X1
STR W1, [X2, #0x3FFC] ; Store 4 bytes from W1 into the address (X2 + (0x3FFC << 2))

```

#### &#x20;   Pre-indexed Immediate Offset:

&#x20;   This mode adds a signed immediate value to a base register and updates the base register with the new address before accessing memory.

```Plain&#x20;Text
LDR X1, [X2, #0x10]!  ; Load 8 bytes from (X2 + 0x10) into X1, then set X2 to (X2 + 0x10)
STR X1, [X2, #-0x10]! ; Store 8 bytes from X1 into (X2 - 0x10), then set X2 to (X2 - 0x10)

```

#### &#x20;   Post-indexed Immediate Offset:

&#x20;   This mode uses a base register for the memory address and then updates the base register with a signed immediate value after the memory access.

```Plain&#x20;Text
LDR X1, [X2], #0x10  ; Load 8 bytes from the address in X2 into X1, then set X2 to (X2 + 0x10)
STR X1, [X2], #-0x10 ; Store 8 bytes from X1 into the address in X2, then set X2 to (X2 - 0x10)

```

#### &#x20;   Register Offset:

&#x20;   This mode adds the contents of a shifted or extended register to a base register to calculate the memory address.

```Plain&#x20;Text
LDR X1, [X2, X3, LSL #3] ; Load 8 bytes from the address (X2 + (X3 << 3)) into X1
STR W1, [X2, X3, LSL #2] ; Store 4 bytes from W1 into the address (X2 + (X3 << 2))

```

#### Literal:

This mode calculates an address in memory within one megabyte of the program counter using a signed offset from the instruction.

```Plain&#x20;Text
LDR X1, =label      ; Load address of the label into X1
LDR X1, [PC, #0x1F] ; Load 8 bytes from the address (PC + 0x1F) into X1

```

### Loading and storing

can be grouped into the 

* dealing with one register (single register)
* dealing with two (register pair)
* atomic

loading and storing operations happen only between the registers and memory (more often than not the RAM) thus we have only two commands for it

* LDR - Load from memory to Register
* STR - Store contents of register TO memory

- When talking about storing we have a convetion to follow apparently
  * `<opcode>{size} R <addr>` where
    * opcode is either `LDR` or `STR`
    * size is either
      * `b` for byte
      * `h` for halfword
      * `sb` for signed byte
      * `sh` for signed halfword
      * `sw` for word
    * R is the register to be



### Branching instructions

Branch instructions allow you to change the address of the next instruction to be executed. They are used to implement loops, if-then structures, subroutines, and other flow control structures. There are five instructions related to branching:

* &#x20; Branch,
* &#x20; Branch to Register,
*  Branch and Link (subroutine call),
* &#x20; Compare and Branch, and
* &#x20; Form program-counter-relative Address.

`b<cond> target`

* where `<cond>` is any of the provided conditional opcodes like `EQ` `NE` `ORR`
* `target` is any operation being performed be it a function or operation result



Branch instructions alter the normal sequential flow of execution by making the processor jump to a different instruction address. This allows for conditional and unconditional changes in the execution path.

**Types of Branch Instructions**

Branch instructions can be broadly categorized into:

* Unconditional Branches:These instructions always result in a jump to a new address, regardless of any conditions. Example:`JMP`(Jump).
* Conditional Branches:These instructions result in a jump only if a specific condition is met. Examples:`BEQ`(Branch if Equal),`BNE`(Branch if Not Equal)



**Components of Branch Instructions**

A typical branch instruction consists of:

* Opcode:Specifies the type of branch (e.g.,`JMP`,`BEQ`).
* Condition (for conditional branches):A specific condition that needs to be met for the branch to occur (e.g., comparison of two values).
* Target Address:The address to which the control should jump if the branch is taken.



**Mechanism of Branching**

* Fetch:The CPU fetches the branch instruction from memory.
* Decode:The CPU decodes the instruction to determine the type of branch and the target address.
* Evaluate Condition (for conditional branches):The CPU evaluates the condition specified in the instruction.
  * If the condition is true, the Program Counter (PC) is updated to the target address.
  * If the condition is false, the PC continues to the next sequential instruction.
* Execute:The CPU executes the jump to the target address if the branch is taken.



### Data processing

| All opcodes seen so far| What it does |
| ------ | ------ |
| adc    | adding with carry |
| add    | adding |
| adr    | address of a label |
| adrp   | address of a page |
| and    | bitwise and |
| asr    | arithmetic shift right |
| b      | branch |
| bic    | bitwise clear |
| bl     | branch and link |
| blr    | branch to link register |
| br     | branch to register |
| cbnz   | compare and branch if not zero |
| cbz    | compare and branch if zero |
| ccmn   | ccmp with immediate |
| ccmp   | ccmp with register |
| cinc   | conditional increment |
| cinv   | conditional invert |
| cls    | count leading sign bits |
| clz    | count leading zeros |
| cmn    | compare and negate |
| cmp    | compare |
| cneg   | conditional negate |
| csel   | conditional select |
| cset   | conditional set |
| csetm  | conditional set mask |
| csinc  | conditional select increment |
| csinv  | conditional select invert |
| csneg  | conditional select negate |
| eon    | exclusive or not |
| eor    | exclusive or |
| ldp    | load pair |
| ldr    | load register |
| ldur   | load register unscaled |
| lsl    | logical shift left |
| lsr    | logical shift right |
| madd   | multiply and add |
| mneg   | multiply and negate |
| mov    | move |
| movk   | move with keep |
| movn   | move with not |
| movz   | move with zero |
| mrs    | move to system register |
| msr    | move from system register |
| msub   | multiply and subtract |
| mul    | multiply |
| mvn    | move with not |
| neg    | negate |
| ngc    | negate with carry |
| nop    | no operation |
| orn    | or not |
| orr    | or |
| ret    | return |
| ror    | rotate right |
| sbc    | subtract with carry |
| sdiv   | signed divide |
| smaddl | signed multiply and add long |
| smnegl | signed multiply and negate long |
| smsubl | signed multiply and subtract long |
| smulh  | signed multiply high |
| smull  | signed multiply long |
| stp    | store pair |
| str    | store register |
| stur   | store register unscaled |
| sub    | subtract |
| svc    | supervisor call |
| tbnz   | test bit and branch if not zero |
| tbz    | test bit and branch if zero |
| tst    | test |
| udiv   | unsigned divide |
| umaddl | unsigned multiply and add long |
| umnegl | unsigned multiply and negate long |
| umsubl | unsigned multiply and subtract long |
| umulh  | unsigned multiply high |
| umull  | unsigned multiply long |

