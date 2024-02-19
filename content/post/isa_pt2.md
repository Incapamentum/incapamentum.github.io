+++
author = "Gustavo Diaz Galeas"
title = "Instruction Set Architecture: Part 2 - Memory Addressing"
date = "2024-02-19"
description = "All your memory belong to us."
thumbnail = "/images/arch/alu.svg"
featureImage = ""
series = "Computer Architecture"
+++

In the previous post, which served as an introduction to instruction set architectures (ISA), some basic terminology was defined which helped frame a discussion regarding different characterizations of architectures. What set each type of architecture apart was namely in the type of _internal storage_ it makes use.

Now, let's take this up one level of abstraction. Internal storage is simply one form of memory. Recall that, in the context of computing in general, this is defined as being a device that stores information. One key question, however, is how is this information stored? More importantly, how does ISAs fit into this?


Often, the key difference between each major characterization is

some basic terminology was defined along with discussing key characteristics that set them apart, namely in the type of internal storage it uses. Thus, this brings along the main point of discussion: memory. Regardless of the choice of architecture, be it stack or register-register, one thing that the ISA _must_ define is how memory addresses are interpreted, and how they are specified.

## Defining Some Key Terminology

Before continuing, let's cover some important terminology that will be useful in continuing a discussion on memory:

- **Bits, Bytes, and Beyond**
  - Where computer memory is concerned, it's often in terms of _bytes_. A byte is defined as being composed of eight _bits_, which is the most basic unit of information. Building from that, we can then define a _half word_ to be 16 bits, and finally a _word_ to be 32 bits. This convention has been the de-facto standard since the introduction of 32-bit machines, but prior to that, a word _has_ been defined to be 16 bits. Modern computers are now 64-bit, but instead of redefining what a word is, a _double word_ was defined that is 64 bits.

- **Alignment**
  - The way how data in memory is arranged at address boundaries matching the size of the data is referred to as _alignment_. As the byte is the smallest unit of memory access, memory addresses are defined to be _byte aligned_. That is to say, each memory address specifies a different byte. Memory alignment is important as it can impact performance. For example: a 32-bit processor will perform memory accesses on a _per word_ basis. If a piece of data is misaligned, then this will take multiple memory accesses.

## Endianness

In storing data as bytes, there can be one of two conventions in _how_ the computer interprets the order. These conventions are known as _Little Endian_ and _Big Endian_.

In Little Endian byte order, byte address `0` is interpreted as the "least" significant byte:

```
+---+---+---+---+
| 4 | 5 | 6 | 7 |   Data value
+---+---+---+---+
  0   1   2   3     Memory address
```

Therefore, the above will be interpreted as `0x7654` in Little Endian.

Conversely, Big Endian will interpret byte address `0` as the "most" significant byte. Using the prior memory layout example, Big Endian will interpret it as `0x4567`.

## Alignment

Recall that alignment refers to the arrangement of data in memory in such a way that the starting address matches the size of the data. As a byte is the smallest unit of memory access, memory addressing is often referred to as being _byte aligned_.

Memory alignment is important because every object that are at word boundaries can be accessed in one memory access. Any misaligned object will take more than one. So from a performance perspective, alignment increases performance by reducing the number of reads/writes and eliminates the need for hardware to do it for us.

As an example, consider the following `struct`:
```C
struct example
{
    char a; // 1 byte
    int b;  // assume 4 bytes
}
```

Without memory alignment, this is how the data will be stored in memory:
```
+---+---+---+---+---+---+---+---+
| a |       b       | X | X | X |
+---+---+---+---+---+---+---+---+
  0   1   2   3   4   5   6   7     Address
```

If the processor attempts to access `b`, it must do so in two: the first will grab the data found in addresses `0x0`, `0x1`, `0x2`, and `0x3` (which constitute a word), and the second will grab data found in addresses `0x4`, `0x5`, `0x6`, and `0x7`. The last three bytes are potentially garbage data and serve no use. Ultimately, the processor will also need to combine the useful bytes from both accesses to obtain the value stored in `b`, which is additional work.

Now, let's consider if the data was aligned:
```
+---+---+---+---+---+---+---+---+
| a | P | P | P |       b       |
+---+---+---+---+---+---+---+---+
  0   1   2   3   4   5   6   7     Address
```

Here, `P` serves as a padding byte to ensure that `b` is aligned at a word boundary. Thus, when the processor accesses `b`, it will do so in one go. Additionally, there won't be any need whatsoever to combine any bytes to obtain the value stored in `b`.