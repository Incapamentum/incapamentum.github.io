+++
author = "Gustavo Diaz Galeas"
title = "Instruction Set Architecture: Part 1"
date = "2023-11-27"
description = "What the hell is an ISA, anyways?"
thumbnail = "/images/arch/alu.svg"
featureImage = "/images/arch/isapt1_banner.png"
series = "Computer Architecture"
+++

Let's start by asking the following questions: what the hell is an **instruction set architecture** (ISA), and why does it matter? For starters, the ISA is the interface between hardware and software. This is of particular importance because it determines what type of computer will be built and how it will perform. In other words, the ISA _determines_ the set of instructions a particular processor can execute. By extension, this also includes the study of different instruction formats, types of addressing mode, and the assembly language support.

The ISA defines many things, including but not limited to program state, semantics, and formatting. These will be covered later, but for now, let's look at _how_ ISAs are typically classified as.

## Defining Some Key Terminology

Define the following:

- ALU
- Accumulator
- Stack(?)
- Memory
- Registers

## Classification of ISAs

For the most part, every processor that has been fabricated work the same: they fetch, decode, and execute instructions. The major thing that differentiates processors is the type of **internal storage** it uses.

The major choices for internal storage are a **stack**, an **accumulator**, or a **set of registers**. Each of these different types come with pros and cons, and will be discussed once each have been covered.

### Stack Architecture

As the name implies, this type of architecture makes use of a stack for its internal storage. As such, two instructions that are supported by default are `push` and `pop`, and they take one operand each: a memory address.

![Stack Architecture](/images/arch/stack_arch.svg)

Arithmetic operations, such as addition and subtraction, are also supported. These types of instructions do not take any operands, as they end up popping the top two entries on the stack, perform the operation, then push the result to the top.

Below is sample instructions demonstrating how two values, stored in memory addresses `A` and `B`, are added together then stored in memory address `C`:

```
push A
push B
add
pop C
```

### Accumulator Architecture

An accumulator is what it sounds like: it is a special type of register that stores intermediate arithmetic results. The type of supported instructions are `load` and `store`, which are for memory access, and arithmetic operations. All instructions in an accumulator architecture take exactly one operand, which are memory addresses. 

![Accumulator Architecture](/images/arch/acc_arch.svg)

Below is a set of sample instructions demonstrating how to add two values. The first value is loaded from memory via address `A`, then the `add` instruction is used to add the value in memory address `B` to the accumulator. The result is effectively `A + B`. This is then stored to memory address `C`, wherefore the value in the accumulator is reset back to 0.


```
load A
add B
store C
```

### Register-Memory Architecture

A register-memory architecture is one of two types of a register architecture. It's defining characteristic is that all instructions support memory access.

![Register-Memory Architecture](/images/arch/reg-mem_arch.svg)

The basic instruction support are `load` and `store` for memory access, and arithmetic operations. The former two take two operands, but the latter set of instructions take three operands, broken down as follows:

```
load <reg>, <reg | addr>
store <reg>, <reg | addr>
ALU <reg1>, <reg2 | addr1>, <reg3 | addr2>
```

For arithmetic instructions, `<reg2 | addr1>` and `<reg3 | addr2>` are source operands, with `<reg1>` being the destination operand.

Below is a set of sample instructions demonstrating the addition of two values. First, the value stored in memory address `A` is loaded into register `R1`. After, the value stored in register `R1` is added with the value in memory address `B`, with the result stored in register `R3`. Finally, the value in register `R3` is then stored into memory address `C`.

```
load R1, A
add R3, R1, B
store R3, C
```

### Register-Register (Load Store) Architecture

A register-register (also known as a load-store) architecture is the second type of a register architecture. It's defining characteristic is that memory access is restricted solely to load and store instructions.

![Register-Register Architecture](/images/arch/reg-reg_arch.svg)

As with a register-memory architecture, it also supports arithmetic operations, which require three operands:

```
load <reg>, <addr>
store <reg>, <addr>
ALU <reg1>, <reg2>, <reg3>
```

Again, similarly as a register-memory architecture, for arithmetic instructions, `<reg2>` and `<reg3>` are source operands with `<reg1>` being the destination operand.

Below is a set of sample instructions demonstrating how two values are added in a load-store architecture. The values stored in memory addresses `A` and `B` are first loaded into registers `R1` and `R2`, respectively. Then, those values are added and stored in the destination register `R3`. Finally, the result is stored from register `R3` to memory address `C`.

```
load R1, A
load R2, B
add R3, R1, R2
store R3, C
```