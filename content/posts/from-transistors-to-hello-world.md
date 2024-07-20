---
title: "From Transistors To Hello World (WIP)"
description: "This article is a not so short introduction to how your machine works under the hood"
date: "2024-07-19"
tags:
- Low Level
- Computer
- Deep Dive
---

Table of content
================
1. [Introduction](#introduction)
2. [Transistors](#transistors)
3. [Logic Gates](#logic-gates)
   - 3.1 [AND gate](#and-gate)
   - 3.2 [OR gate](#or-gate)
   - 3.3 [NOT gate](#not-gate)
   - 3.4 [NAND gate](#nand-gate)
   - 3.5 [NOR gate](#nor-gate)
   - 3.6 [XOR gate](#xor-gate)
   - 3.7 [XNOR gate](#xnor-gate)
4. [Adders](#adders)
5. [Registers](#registers)
6. [The ALU](#the-alu)
7. [The CPU](#the-cpu)
6. [Memory](#memory)
7. [Assembly](#assembly)
8. [Operating System](#operating-system)
9. [Hello World](#hello-world)
10. [Conclusion](#conclusion)

---

Introduction
============
This article is a not so short introduction to how your machine works under the hood. We will start from the very basic building blocks of a computer and build up to a simple program that prints "Hello World" to the screen. With that said don't worry if you are not well read on how hardware/software works i will be explaining it in as simple of way as i can (no promises).

It all starts with this tiny little thing called a transistor.

Transistors
===========
![transistor](/transistor-to-helloworld/transistor.jpg)
> A transistor is a semiconductor device used to amplify or switch electronic signals and electrical power. 

is the wikipedia definition of a transistor. But what does that mean?

Think of it as a nerve in your brain just like how a nerve can fire an electrical signal, a transistor can also fire an electrical signal. But unlike a nerve a transistor can only fire 0 or 1, this is because a transistor has 3 states, off, on and in between (doesn't get mentioned often for simplicity sake). When a transistor is off it is said to be in a low state and when it is on it is said to be in a high state. The in between state is when the transistor is transitioning from low to high or high to low.

With only 0 and 1 (which is called Binary), We can represent numbers 0 and 1 by turning the transistor off and on respectively, that is about as much data we can represent with one transister. but how do we represent other numbers? simple we just use more transistors. 

In how many orders can we represent numbers with n transistors? 2^n. This is because each transistor can be in 2 states, off or on. So with 1 transistor we can represent 2 numbers, 0 and 1. With 2 transistors we can represent 4 numbers, 00, 01, 10, 11. With 3 transistors we can represent 8 numbers, 000, 001, 010, 011, 100, 101, 110, 111. And so on.

Just like addition in decimal we add binary numbers by adding the bits from right to left and carrying over when the sum is greater than 1. For example 1 + 1 = 10 in binary.

<!-- An Ascii representation of adding 0 + 1, until 8 -->
```plaintext
   0
+  1
-----
   1

   1 <= in decimal
```

```plaintext
   1
+  1
----- <- we carry over the 1 to the next bit
  10

2 <= in decimal
```

```plaintext
  10
+  1
-----
  11

3 <= in decimal
```

Alright now that we know we can add binary numbers to infinity using our knowledge of basic addition, how do we instruct the computer to do this? 🥁🥁🥁



Logic Gates
===========
Logic gate simply put is a device that takes in one or more binary inputs and gives out a binary output based on a certain rule. There are 7 basic logic gates, AND, OR, NOT, NAND, NOR, XOR, XNOR.

It is the machine's way of making decisions, think of it as basic rules. For example the AND gate takes in 2 inputs and gives out 1 output. The output is 1 if and only if both inputs are 1. Otherwise the output is 0.

### AND gate
--------
The AND gate takes in 2 inputs and gives out 1 output. The output is 1 if and only if both inputs are 1. Otherwise the output is 0.

| A    | B     | A AND B |
|---------------- | --------------- | --------------- |
| 0    | 0     | 0 |
| 0    | 1     | 0 |
| 1    | 0     | 0 |
| 1    | 1     | 1 |

### OR gate
--------
The OR gate takes in 2 inputs and gives out 1 output. The output is 1 if at least one of the inputs is 1. Otherwise the output is 0.

| A    | B     | A OR B |
|---------------- | --------------- | --------------- |
| 0    | 0     | 0 |
| 0    | 1     | 1 |
| 1    | 0     | 1 |
| 1    | 1     | 1 |

### NOT gate
--------
The NOT gate takes in 1 input and gives out 1 output. The output is the opposite of the input.

| A    | NOT A |
|---------------- | --------------- |
| 0    | 1 |
| 1    | 0 |

### NAND gate
--------
The NAND gate takes in 2 inputs and gives out 1 output. The output is 0 if and only if both inputs are 1. Otherwise the output is 1.

| A    | B     | A NAND B |
|---------------- | --------------- | --------------- |
| 0    | 0     | 1 |
| 0    | 1     | 1 |
| 1    | 0     | 1 |
| 1    | 1     | 0 |

### NOR gate
--------
The NOR gate takes in 2 inputs and gives out 1 output. The output is 0 if at least one of the inputs is 1. Otherwise the output is 1.

| A    | B     | A NOR B |
|---------------- | --------------- | --------------- |
| 0    | 0     | 1 |
| 0    | 1     | 0 |
| 1    | 0     | 0 |
| 1    | 1     | 0 |

### XOR gate
--------
The XOR gate takes in 2 inputs and gives out 1 output. The output is 1 if and only if one of the inputs is 1. Otherwise the output is 0.

| A    | B     | A XOR B |
|---------------- | --------------- | --------------- |
| 0    | 0     | 0 |
| 0    | 1     | 1 |
| 1    | 0     | 1 |
| 1    | 1     | 0 |

### XNOR gate
--------
The XNOR gate takes in 2 inputs and gives out 1 output. The output is 1 if and only if both inputs are the same. Otherwise the output is 0.

| A    | B     | A XNOR B |
|---------------- | --------------- | --------------- |
| 0    | 0     | 1 |
| 0    | 1     | 0 |
| 1    | 0     | 0 |
| 1    | 1     | 1 |



![computer chip](/transistor-to-helloworld/computer_chip.png)




<!-- It is composed of semiconductor material usually with at least three terminals for connection to an external circuit. A voltage or current applied to one pair of the transistor's terminals controls the current through another pair of terminals. Because the controlled (output) power can be higher than the controlling (input) power, a transistor can amplify a signal. Today, some transistors are packaged individually, but many more are found embedded in integrated circuits. -->
