---
layout: default
title: Lab01
nav_order: 1
parent: Assignments
permalink: /_assignments/lab01
---

# Lab01 Number Conversion in C

## Requirements
1. You will implement a base conversion tool called lab01. It converts numbers expressed in bases 2, 10, and 16 into the other two bases. Examples:

    $ ./lab01 10 -o 2
    0b1010
    $ ./lab01 0xFF -o 10
    255
    $ ./lab01 0b11011110101011011011111011101111 -o 16
    0xDEADBEEF
    $ ./lab01 0b11111111111111111111111111111111 -o 10
    4294967295
    $ ./lab01 0x0000000B -o 10
    11
    $ ./lab01 0b123 -o 2
    Bad input

1. You must implement the base conversions yourself, without using C library `printf(%d)`, `printf("%x")`,  `scanf()`, or `atoi()`
1. You must provide a `Makefile` which builds an executable called `lab01`

## Given
We will review processing command line arguments in C

Pseudocode for `uint32_t string_to_int(char *str)`

    init retval to 0
    init placeval to 1 (anything to the 0 power is 1)
    loop over str from the highest index down to 0
        calculate the integer corresponding to the character at that index	
        calculate the value of that place by multiplying the integer * placeval
        add the value to the retval
        update to placeval to placeval * base
    return the return value

Pseudocode for `void int_to_string(uint32_t value, char *str, int base)`

    init buffer to empty
    while value != 0
        quot = value / base
        rem = value % base
        calculate the character value of rem
        append the character value to the buffer
        value = quot
    copy buffer into str in reverse order

## Rubric

Points determined by lab01 autograder tests.

Get autograder here:
[https://github.com/phpeterson-usf/autograder](https://github.com/phpeterson-usf/autograder)

Get tests repo here:
[https://github.com/USF-CS315-F23/tests](https://github.com/USF-CS315-F23/tests)
