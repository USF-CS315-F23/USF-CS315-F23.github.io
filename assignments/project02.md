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

## Given

1. The starter repo contains C implementations for each of the programs
1. Test cases are available in [https://github.com/USF-CS315-F23/tests](https://github.com/USF-CS315-F23/tests)

## Exam-like Problems

## Rubric

1. 80 points: automated test cases
1. 20 points: exam-like problems

## Code quality

Points will be deduction if you repo is not clean, that if it contains build artifacts like executables or `.o` files.

Just like with C code, make sure that you format you Assembly code consistently. Use consistent eindentation (spaces not tabs), consistent label formatting, and consistent use of newlines. Points will be deducted for any code quality issues, but any points deducted can be earned back.

