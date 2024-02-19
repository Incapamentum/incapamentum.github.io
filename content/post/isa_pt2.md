+++
author = "Gustavo Diaz Galeas"
title = "Instruction Set Architecture: Part 2 - Memory Addressing"
date = "2024-02-19"
description = "All your memory belong to us."
thumbnail = "/images/arch/alu.svg"
featureImage = ""
series = "Computer Architecture"
+++

In the previous post, which served as an introduction to instruction set architectures (ISA), some basic terminology was defined which helped frame a discussion regarding different characterizations of architectures. What set each type of architecture apart was namely in the type of _internal storage_ it uses.

Now, let's take this up one level of abstraction. Internal storage is simply one form of memory. Recall that, in the context of computing, this is defined as being a device that stores information. Thus, this brings us to the following question: how is this information stored? More importantly, how does this tie into the overall discussion on ISAs?

## Defining Some Key Terminology

Before continuing, let's cover some important terminology that will be useful moving forward:

- **Bits, Bytes, and Beyond**
  - Where computer memory is concerned, it's often in terms of _bytes_. A byte is defined as being composed of eight _bits_, which is the most basic unit of information. Building from that, we can then define a _half word_ to be 16 bits, and finally a _word_ to be 32 bits. This convention has been the de-facto standard since the introduction of 32-bit machines, but prior to that, a word _has_ been defined to be 16 bits. Modern computers are now 64-bit, but instead of redefining what a word is, the term of _double word_ is used to denote that it is 64 bits in length.

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

## Questions Answered

Recall that, in the intro of the post, I had posed two questions. The first asked how information is stored in memory. The answer to this question came from defining what a _byte_ is, and extending it both below and above it.

The second question concerned itself with how does this all tie in to ISAs? This was answered through two topics: endianness and alignment. In short, an ISA _must_ define how memory is addressed (through alignment) and how to interpret them (through endianness).