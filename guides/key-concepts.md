---
layout: default
title: Key Concepts
nav_order: 7
parent: Guides
permalink: /guides/key-concepts
---

# Key Concepts from Labs and Projects

## Lab01 - Hello World in the RISC-V Dev Env

- Console based editing: micro or vim
- git and GitHub usage
- Makefiles
- Hello World in C
- Autograder

## Project01 - RISC-V Access and Number Conversion in C

- Dev environment setup
  - Local VM using qemu-system-riscv64
  - Remote VM on euryale
  - ssh keys, ssh `config`, ssh `authorized_keys`
  - Shell configuration
    - bash: `~/.profile`, `~/.bash_profile`, `~/.bashrc`, `~/.bash_aliases`
    - zsh: `~/.zprofile`, `~/.zshrc'
- C Basics
  - Functions
  - Data (global, stack, heap)
  - Statements and expressions
  - Data types
    - Primitives - int, char, float, uint8_t, uint32_t, int32_t, uint64_t, int64_t
    - Composit - arrays and structs
    - Pointers - int *, char *, uint32_t *
  - Pointer operations
    - & address of, e.g., `int x; int *p; p = &x;`
    - * dereference, e.g., `x = *p;`
  - Strings in C
    - Array of chars (bytes)
    - Null `\0` terminated
    - ASCII character codes
  - The C stack
    - Local variables
  - Signed (int, int32_t) vs unsigned (unsigned int, uint32_t) values
- Command line arguments with `int argc` and `char *argv`
  - Layout of `argv` array in memory
  - Array of pointers first, `argv[0]`, `argv[1]`, `argv[2]`, `NULL (0)`
  - String arrays second, `./foo` + `\0`, `-p` + `\0`, etc.
- Parsing command line arguments
- Number systems
  - Decimal (base 10, digits: `0`, `1`, `2`, `3`, `4`, `5`, `6`, `7`, `8`, `9`)
  - Binary (base 2, digits: `0`, `1`, C literal prefix: `0b`)
  - Hexadecimal, "hex" (base 16: digits: `0`, `1`, `2`, `3`, `4`, `5`, `6`, `7`, `8`, `9`, `A`, `B`, `C`, `D`, `E`, `F`, C literal prefix: `0x`)
    - `A` = 10, `B` = 11, `C` = 12, `D` = `13`, E = `14`, F = `15`
- base conversions
  - C string as decimal, binay, hexadecimal to int
    - Place value: `2 * (10^2) + 5 * (10^1) + 4 * (10^0)`   
  - int to C string as decimal, binary, hexadecimal
    - Modulus to find digit
    - Divide to get value for next digit
    - Repeat until divide gives 0   

## Lab02 - Introduction to RISC-V Assembly Programming

- Instructions, registers, labels, directives
  - See [RISC-V Assembly Guide]({{ site.url }}/guides/riscv-asm.html)
- Assembling vs compiling
  - Assembling (as) assembly source (.s) to object code (.o)
  - Compiling (gcc) c source (.c) to object code (.o)
  - The object files must be linked, we can use gcc for this
    - `as -o foo.o foo.s`
    - `gcc -o main main.c foo.o`      
  - The C compiler can also assemble
    - `gcc -o main main.c main.s`
- Instructions have a name (mnemonic), like `add` or `mul`
- Assembly programming model
  - Processor has 32 registers plus the `PC` registers
    - `x0`, `x1`, ... `x31`
    - ABI names: `sp`, `ra`, `a0`, `a1`, ..., `t0`, `t1`, ..., `s0`, `s1`, ...
  - The `PC` points to next instruction in memory to execute
  - Load instruction from memory into processor
  - Execute instruction, usually updates a register value, but can also update memory
  - Update `PC`, often just the next instruction `PC + 4`, but can also be a diffenent address in the case of a branch or jump (control instruction)
- Most instructions have three operands
  - One target (destination)
  - Two source
  - `add t0, t1, t1` means `t0 = t1 + t2`
- On RISC-V 64 bit registers are 64 bits (8 bytes)
- Data processing instructions
- Immediate values
- Control instructions
  - conditional branchs: `beq`, `bne`, `blt`, `bge`
  - jumps: `j`
  - return: `ret`
- Assembly arguments passed in `a0`, `a1`, `a2`, `a3`, etc.
- Return value is put into `a0`
- Basic assembly function
  ```text
  .global func_name
  func_name:
      add a0, a0, a1
      ret
  ```
- if/then/else in assembly
- for loops in assembly
- Array access in assembly
  - lw (load word) `lw t0, (a0)` means `t0 = *a0`
  - Load word at address `a0` into register `t0`
  - Pointer based array access
    - To get to next element in array of words (ints): `addi a0, a0, 4`

## Project02 - RISC-V Assembly Language

- RISC-V ABI Function Calling Conventions
  - Only one set of registers (32), so we need a way to coordinate usage between functions
  - Caller-saved registers (`a0`, `a1`, ..., `a7`, `t0`, `t`, ... `t6`)
    - These must be preserved on the stack by the caller of a function
    - You only need to preserve the registers you will be using after the function call
  - Callee-saved registers (`sp`, `ra`, `s0`, `s1`, ... `s11`)
    - These must be preserved on the stack by the callee, on entry to the function.
    - They should be restored on exit.
    - The stack pointer is preserved by subtracting (stack allocation) on function entry and adding (stack deallocation) on function exit the name number of bytes.
    - The `sp` should be a multiple of 16 (for performance compatibility with some instructions)
- Functions that don't call other functions do not need to allocate stack space.
  - Only need stack space if the function uses caller-saved registers
- Function call template when using stack:
  ```text
  .global func_name
  func_name:
      addi sp, sp, -N    # Allocate N bytes on stack, N must be a multiple of 16
      sd ra, (sp)         # Save ra onto stack
                          # Save any callee-saved registers if needed
      ...
      ld ra, (sp)
      addi sp, sp, +N
      ret
  ```
- Indexed-based array access
  - Assume `t0` is `int i`, `a0` is base address of array `int arr[]`
  ```text
  li t1, 4
  mul t1, t1, t0
  add t1, a0, t1
  lw t2, (t1)
  ```
  - Altneratively use a shift instead of multiply
  ```text
  slli t1, t0, 2
  add t1, a0, t1
  lw t2, (t1)
  ```
- Recursive functions
  - Nothing special, just that the func calls itself
  - Same rules apply
  - Presere any caller-saved registers on stack before the recursive call
  - Restore any caller-saved registers from stack after the recursive call
  - If a recursive function simply returns after the recursive call, then it is called *tail recursive* and the recursive call can be turned into a jump.
 
 
## Project03 - RISC-V Assembly Language Part 2

- Working with C strings in assembly
- Use `lb` (load byte) and `sb` (store byte) to access characters in a string
- Calling C functions from Assembly
  - Just add a `.global func_name` for the function you want to use
  - E.g., `.global strlen`
  - Then set arg registers and use `call` instruction as normal
  - Make sure to preserve any caller-saved registers before call 
- Memory
  - Byte addressable
  - Smallest addressable unit is a byte (8 bits)
  - A int (word) is 4 bytes, need to decide how to layout a word in memory
  - Two byte orderings
    - Big endian (put the most significant byte first)
    - Little endian (put the least significant byte first)
    ```text
    int x = 0xFFAA1122

    4 |    |   4 |    |
    3 | 22 |   3 | FF |
    2 | 11 |   2 | AA |
    1 | AA |   1 | 11 |
    0 | FF |   0 | 22 |
    Big        Little
    Endian     Endian
    ```
- Two's Complement (2's Complement)
  - Need a way to represent negative integer values
  - To get the two's complement negative representation of a positive value:
    - twos = invert_bits(val) + 1
  - 3-bit two's complement values
  ```text
  Bin Dec
  000   0
  001   1
  010   2
  011   3
  100  -4
  101  -3
  110  -2
  111  -1
  ```
  - Two's complement properties
    - The msb (most significant bit) is the sign bit (0 is pos, 1 is neg)
    - Only one representation of 0
    - Because of this, there is one more neg value than pos values
    - Simple grade school arithmetic work on two's complement numbers
- Bit manipulation and bitwise operators
  - Zero (0) / One (1)
    - Other names: unset/set, low/higg, off/on, false/true
  - A byte is 8 bits
    ```text
    msb | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 | lsb
    ```
    - msb - most significant bit
    - lsb - least significant bit
  - A word is 4 bytes or 32 bits
  ```text
  msb | 31      24|23      16|15       8|7        0| lsb
      |   byte 3  |  byte 2  |  byte 1  |  byte 0  |
  ```
  - Bitwise Operaters (op, C, ASM)
     - `AND` ,  `&`  , `and/andi`
     - `OR`  ,  `|`  , `or/ori`
     - `NOT` ,  `~`
     - `XOR` ,  `^`  , 
     - Shift Left Logical
       - Shift x to the left by n bits
       - Fill lower bits with 0
       - `uint32_t x`
       - `x << n`
       - `sll/slli`
     - Shift Right Logical
       - Shift x to the right by n bits
       - Fill upper bits with 0
       - `uint32_t x`
       - `x >> n`
       - `srl/srli`
     - Shift Right Arithmetic
       - Shift x to the right by n bits
       - Fill upper bits with sign bit value
       - `int32_t x`
       - `x >> n`
       - `sra/srai`
  - Masking
    - It is often need to pull out a sequence of bits from a larger value
      - To get a sequence of bits:
      - Shift the bits to the far right
      - Mask the bits to zero out any upper bits we don't want
      - Example: assume we want the middle 4 bits of an 8 bit value (bits 2-5)
      ```text
      uint8_t x = 0b11011010;
      uint32_t v = x >> 2     // Shift the 4 bits to the beginning
      uint32_t v = v & 0b1111 // Mask off upper bits remaining in v
      ```
   - Combine individual bit sequences with right shift and OR (`|`)
   - Sign extension
     - Assume you have a 4-bit two's complement number you want to sign extend to be a 32-bit two's complement number.
     ```text
     int32_t v = 0b1110 // -2 in 4 bits
     v = (v << 28) >> 28;
     ```
     - Shift all the way to the left and then do shift right arithmetic all the way back
