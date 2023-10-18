---
layout: default
title: Lab04
nav_order: 8
parent: Assignments
permalink: /assignments/lab04
---

# Introduction to Digital Design

## Schematics due Mon Oct 23rd by 11:59pm in your Lab04 GitHub repo

## Background
We are now learning the basics of digital logic and digital circuits with the goal of learning enough to build a working computer processor. We will use the Digital application to build and simulate digital circuits:
[https://github.com/hneemann/Digital](https://github.com/hneemann/Digital). 
For this lab your are going to build and simulate some simple circuits.

## Part 1: LEDs
1. You will build a circuit in with two inputs and 4 LED output that shows how 4 basics gates work: NOT, AND, OR, and XOR. 
1. The LED will turn red when the input is high (1) and black when in the input is low (0). 
1. Here is a screen shot of the circuit you need to build and simulate. Note that you need to add labels to the inputs (A and B):

![lab04-part1](lab04-part1.png)

## Part 2: Max2
Consider the C code fragment below:

    if (a > b) {
        r = a;
    } else {
        r = b;
    }

1. We will assume that a, b, and r are 2-bit values. That is, the only values a, b, and r can be are 0, 1, 2, or 3. 
1. Your job is to use sum-of-products to come up with a Boolean algebra equation and a circuit that will compute the max value r (which is also a 2-bit value). 
1. Your inputs will be `a1`, `a0`, `b1`, `b0` and your outputs will be `r1` and `r0`. So, you will have two Boolean equations, one for `r1` and one for `r0`.  
1. Since there are 4 inputs you will have 2^4 (16) rows in your truth table. 
1. Submit a working circuit that correctly produces the max value of the two inputs. You do not need to submit the truth table or Boolean algebra equation.
1. Your circuit must have input names `a1`, `a0`, `b1`, and `b0`. The file must be called `max2.dig`

## Part 3: 1-bit full adder
1. In lecture, we built a 1-bit half adder (that is, an adder which does not have a carry-in) and showed the Boolean Algebra equation for a 1-bit full adder. 
1. You will build a 1-bit full adder in Digital, including a carry-in (`cin`) and a carry-out (`cout`).
Your circuit must have inputs named `a`, `b`, and `cin`, and outputs named `sum` and `cout`. Your file must be named `1-bit-full-adder.dig`
1. Here are the truth table and sum-of-products equations for `sum` and `cout` 

![lab04-part3](lab04-part3.png)

## Part 4: 8-bit Ripple Carry Adder
1. You will implement an 8-bit Ripple Carry Adder with two 8-bit inputs, a 1-bit Carry In, one 8-bit result output, and a 1-bit Carry Out.
1. Your circuit must have inputs named `a`, `b`, and `cin`, and outputs named `sum` and `cout`. Your file must be called `8-bit-ripple-carry-adder.dig`

## Rubric
1. 33 pts: max2
1. 33 pts: 1-bit full adder
1. 34 pts: 8-bit full adder
1. The LED circuit is just a warmup and will not be graded
