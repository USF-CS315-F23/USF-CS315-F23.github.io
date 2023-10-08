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
  - String arrays second, './foo` + `\0`, `-p` + `\0`, etc.
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
    - The stack pointer is preserve by subtracting (stack allocation) on function entry and adding (stack deallocation) on function exit the name number of bytes.
    - The `sp` should be a multiple of 16 (for performance compatibility with some instructions)
  


    
    

