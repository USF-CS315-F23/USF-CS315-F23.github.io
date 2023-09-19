---
layout: default
title: Project02
nav_order: 4
parent: Assignments
permalink: /assignments/project02
---

# RISC-V Assembly Language

## Due Mon Sep 18th by 11:59pm in your Project02 GitHub repo

Note: Project02 will be graded offline (not interactively)

## Exam problems due Wed Sep 20th by 11:59pm in your Project02 GitHub repo

## Requirements

1. You will develop RISC-V assembly language implementations of the following problems, and print the results to ensure that both the C implementation and your RISC-V implementation compute the correct answer.
1. Your executables must be named as follows, and must be compiled with a `Makefile`
1. We will test your projects using autograder
1. Note that in all your programs below you must follow the RISC-V function calling conventions that we cover in class.
1. Remeber to remove the line `# YOUR CODE HERE` from the starter code.


**max3**

Given three signed integer parameters, find the largest by comparing the first two, and then comparing the larger of those with the third. That is, max3_s must be implemented using two calls to `max2_s`.

    $ ./max3 2 4 6
    C: 6
    Asm: 6

**swap**

Given an array of integers, swap element at index i with the element at index j:

    ./swap <i> <j> <a0> <a1> <a2> ...

    $ ./swap 0 1 5 4 3 2 1
    C: 4 5 3 2 1
    Asm: 4 5 3 2 1
    ./swap 4 3 11 22 33 55 66 77
    C: 11 22 33 66 55 77
    Asm: 11 22 33 66 55 77

**sort**

Given the address of an array of unsigned integers, and the length of the array, sort the input array in increasing order (largest to smallest) in place using the given C implementation of insertion sort. Your `sort_s` implementation must call `swap_s`.

    $ ./sort 10 30 20
    C: 10 20 30
    Asm: 10 20 30

**fibrec:** Recursive Fibonacci number generator. Examples:

    $ ./fib_rec 10
    C: 55
    Asm: 55

    $ ./fib_rec 20
    C: 6765
    Asm: 6765

## Given

1. The starter repo contains C implementations for each of the programs
1. Test cases are available in [https://github.com/USF-CS315-F23/tests](https://github.com/USF-CS315-F23/tests)

## Exam-like Problems

Put the solutions to the following problem in a file called `problems.pdf` in your Project02 GitHub Repo. You can typeset your solutions or you can write your solutions by hand and scan or take a photo of your work. Just be sure to put your solution in a single file called `problems.pdf`.

### Question 1 - RISC-V Snippet 1

Assume `a0 = 1`, `a1 = 2`, and `a2 = 3`. What is teh value of `a2` after executing this snippet:

    add a0, a0, a0
    add a1, a1, a2
    mul a2, a1, a0

### Question 2 - RISC-V Snippet 2

Assume `a0 = 0`, `a1 = 3`. What is the value of a0 after executing this snippet?

        addi a0, a0, 1
        j boo
    foo:
        addi a0, a0, a1
        j goo
    boo:
        addi a0, a0, 2
        beq a0, a1, foo
    goo:
        addi a0, a0, 1

### Question 3 - RISC-V Snippet 3

Assume `a0 = 0`, `a1 = 2`, and `a2 = 0`. What is the value of a0 after executing this snippet? How many instructions are executing in the snippet below? Note that labels such as `loop:` and `loopend:` do not count as instructions. Also assum beq counts as one instruction whether or not the branch is taken.

        li a3, 0
    loop:
        beq a1, a2, loopend
        add a3, a3, a1
        addi a1, a1, -1
        j loop
    loopend:
        mv a0, a3

## Rubric

1. 80 points: automated test cases
1. 20 points: exam-like problems

## Code quality

Points will be deduction if you repo is not clean, that if it contains build artifacts like executables or `.o` files.

Just like with C code, make sure that you format you Assembly code consistently. Use consistent indentation (spaces not tabs), consistent label formatting, and consistent use of newlines. Points will be deducted for any code quality issues, but any points deducted can be earned back.

