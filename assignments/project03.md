---
layout: default
title: Project03
nav_order: 5
parent: Assignments
permalink: /assignments/project03
---

# RISC-V Assembly Languague Part 2

## Due Mon Sep 25th by 11:59pm in your Project03 GitHub repo

## Project03 Interactive Grading will take place on Tuesday Sep 26th.

## Exam problems due Wed Sep 27th by 11:59pm in your Project03 GitHub repo

## Requirements

1. You will develop RISC-V assembly language implementations of the following problems, and print the results to ensure that both the C implementation and your RISC-V implementation compute the correct answer.
1. Your executables must be named as follows, and must be compiled with a `Makefile`
1. We will test your projects using autograder
1. Note that in all your programs below you must follow the RISC-V function calling conventions that we cover in class.
1. Your solutions must follow the logic and implement the same functions as given in the C code.
1. Remeber to remove the line `# YOUR CODE HERE` from the starter code.


**rstr:** Reverse a string

    $ ./rstr a
    C: a
    Asm: a

    $ ./rstr FooBar
    C: raBooF
    Asm: raBooF

    $ ./rstr "CS315 Computer Architecture"
    C: erutcetihcrA retupmoC 513SC
    Asm: erutcetihcrA retupmoC 513SC
 
**rstr_rec:** Reverse a string recursively

    $ ./rstr_rec a
    C: a
    Asm: a

    $ ./rstr_rec FooBar
    C: raBooF
    Asm: raBooF

    $ ./rstr_rec  "CS315 Computer Architecture"
    C: erutcetihcrA retupmoC 513SC
    Asm: erutcetihcrA retupmoC 513SC

**eval:** Evaluate simple expressions recursively (this is the hardest problem)

    ./eval "4+32"
    C: 36
    Asm: 36

    ./eval "4+32/2"
    C: 20
    Asm: 20 

    ./eval "(4+32)/2"
    C: 18
    Asm: 18

    ./eval "(2*100)+(4*10)+(5*1)"
    C: 245
    Asm: 245
    
**pack_bytes**

Given four 1-byte values (unsigned), you will combine them into a single 32-bit signed integer value.

    $ ./pack_bytes
    usage: pack_bytes <b3> <b2> <b1> <b0>

    $ ./pack_bytes 1 2 3 4
    C: 16909060
    Asm: 16909060

    $ ./pack_bytes 255 255 255 255
    C: -1
    Asm: -1

**unpack_bytes**

Given a signed 32 bit integer value, unpack each byte as an unsigned value:

    $ ./unpack_bytes 1000000
    C: 0 15 66 64
    Asm: 0 15 66 64

    $ ./unpack_bytes -2
    C: 255 255 255 254
    Asm: 255 255 255 254

**get_bitseq** (this will be developed in class)

Given a 32-bit unsigned integer, extract a sequence of bits and return as an unsigned integer. Bits are numbered from 0 at the least-significant place to 31 at the most-significant place. Here is the usage:

`get_bitseq <value> <start_bit> <end_bit>`

    $ ./get_bitseq 94117 12 15
    C: 6
    Asm: 6

    $./get_bitseq 94117 4 7
    C: 10
    Asm: 10

**get_bitseq_signed**

Given a 32-bit unsigned integer, extract a sequence of bits and return as a signed integer. Bits are numbered from 0 at the least-significant place to 31 at the most-significant place. When computing the signed return value you can assume the end_bit is the most significant bit of the signed bit range. You need to sign-extend this value to become a 32-bit 2's complement signed value. Here is the usage:

`get_bitseq_signed <value> <start_bit> <end_bit>`

    $ ./get_bitseq_signed 94117 12 15
    C: 6
    Asm: 6

    $./get_bitseq_signed 94117 4 7
    C: -6
    Asm: -6


## Exam-like Problems

Put the solutions to the following problems in a file called `problems.pdf` in your Project03 GitHub Repo. You can typeset your solutions or you can write your solutions by hand and scan or take a photo of your work. Just be sure to put your solution in a single file called `problems.pdf`.

### Question 1 - RISC-V Assembly

Consider the following RISC-V assembly code, then answer the following questions.

```text
.global swap_s
.global sort_s

/* sort_s sorts an array of 32-bit integers in-place,
   in asceding order

   a0 - int arr[]
   a1 - int len

   t0 - int i;
   t1 - int j;

*/

sort_s:
    addi sp, sp, -64
    sd ra, (sp)
    li t0, 1

floop:
    bge t0, a1, fdone
    mv t1, t0

wloop:
    ble t1, zero, wdone
    li t3, 4
    mul t4, t1, t3
    add t5, a0, t4
    addi t6, t5, -4
    lw t5, (t5)
    lw t6, (t6)
    ble t6, t5, wdone

    sd a0, 8(sp)
    sd a1, 16(sp)
    sd t0, 24(sp)
    sd t1, 32(sp)

    mv a1, t1
    addi a2, t1, -1
    call swap_s

    ld a0, 8(sp)
    ld a1, 16(sp)
    ld t0, 24(sp)
    ld t1, 32(sp)

    addi t1, t1, -1
    j wloop

wdone:
    addi t0, t0, 1
    j floop

fdone:
    ld ra, (sp)
    add sp, sp, 64
    ret
```

Which caller-saved registers are preserved in this function, if any?

Which callee-saved registers are preserved in this function, if any?

Are there caller-saved registers that are used but not preserved? If so, why is this okay?

How many bytes of the stack are actually used by this function?

Does this function use pointer-based array access or indexed-based array access?


### Question 2 - C to Assembly

Consider the following C function. Provide and English description of what this function does and provide the RISC-V implementation of this function.

```text
int count_rec_c(char *str, char c) {
    int addval = 0;

    if (str[0] == '\0') {
        return 0;
    } else {
        if (str[0] == c) {
            addval = 1;
        }
        return addval + count_rec_c(&str[1], c);
    }
}

```

### Question 3 - RISC-V Machine Code

Lets assume that we have a new RISC-V instruction format called the `x-type`:

```text
 31                 20 19       15 14    12 11       7 6       0
|   imm[5:0|11:6]     |    rs1    | funct3 |    rd    |  opcode |
```

Assume you have this instruction word in a C `uint32_t` variable called `iw`. Write a C code snippet that can construct the a 32 bit signed immediate value from `iw`, which is a `int32_t` type called `imm`. Your code snippet should just use C variables and expressions, no function calls. You cannot use `get_bits()` or `sign_extend()`.



## Rubric

1. 80 points: automated test cases
1. 10 points: interactive grading questions
    1. Design decisions (why did you choose to do it this way?)
    1. Explanation of your implementation (show me how X works?)
    1. Problems encountered and debugged
    1. Single-stepping a program with gdb
1. 10 points: exam-like problems

## Code quality

Points will be deduction if you repo is not clean, that if it contains build artifacts like executables or `.o` files.

Just like with C code, make sure that you format you Assembly code consistently. Use consistent indentation (spaces not tabs), consistent label formatting, and consistent use of newlines. Points will be deducted for any code quality issues, but any points deducted can be earned back.
