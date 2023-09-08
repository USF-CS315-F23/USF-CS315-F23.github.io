---
layout: default
title: Lab02
nav_order: 3
parent: Assignments
permalink: /assignments/lab02
---

# Introduction to RISC-V Assembly Programming

## Deliverables due Mon Sep 11th by 11:59pm in your Lab02 GitHub repo

## Links

Tests: [https://github.com/USF-CS315-F23/tests](https://github.com/USF-CS315-F23/tests)

Autograder: [https://github.com/phpeterson-usf/autograder](https://github.com/phpeterson-usf/autograder)


## Requirements

1. You will develop RISC-V assembly language implementations of the following arithmetic problems. You will be given the C implementations, you need to write the RISC-V implementations. 
1. Your executable must be compiled with a Makefile
1. Before you add files to your repo, do a `$ make clean` so you don't add/commit build products like executables or .o files
1. We will test the labs using autograder

**add4:** Adds four 32-bit integers together and returns the result. Example:

    $ ./add4 1 2 3 4
    C: 10
    Asm: 10

**quadratic:** Runs the quadratic equation (ax^2 + bx + c) on the 32-bit integers x, a, b, c and returns the result. Example:

    $ ./quadratic 4 3 2 1
    C: 57
    Asm: 57

**min:** Calculates the smaller of two 32-bit integers and returns its value. Example:

    $ ./min 2 3
    C: 2
    Asm: 2

**sum_array** Sums the integer values in an array. 

    $ ./sumarr 1 2 3 4 5
    C: 15
    Asm: 15

## Given

The Lab02 GitHub repo will contain starter code including a Makefile, the C versions of the functions above, and empty `.s` files for you to fill in.

## Exam Like Problems

### Question 1 - Hex to Decimal



## Rubric

1. Your lab will receive the score indicated by the autograder
1. To get the test cases, `git pull` in the tests repo
