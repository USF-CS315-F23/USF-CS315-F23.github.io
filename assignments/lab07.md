---
layout: default
title: Lab07
nav_order: 14
parent: Assignments
permalink: /assignments/lab06
---

# Exam-like Problems

## Due in your Lab07 repo by Sunday, December 10th, 11:59pm

You only need to attempt the problems, you do not need to provide correct answers (but it best if you try to come up with a correct answer :-)

## Name your solutions file: problems.pdf

Failure to name your file exactly like this will result in a 0.

## Question 1 - Sum-of-Products

Consider implementing the Majority function of 3 1-bit inputs, `r = majority(a, b, c)`. The the value of the Majority function is true if and only if the majority (at least two) inputs are true. For example:

```text
majority(1, 0, 1) = 1
majority(0, 1, 0) = 0
```

Build a truth table for this function. Next, derive the sum-of-products boolean algebra equation for this function. Finally, draw a circuit that implements this function in terms of AND, OR, and NOT gates.

## Question 2 - Digital Design

Consider the following circuit called Max2, that determines the maximum of two 64-bit inputs. That is, the circuit take inputs A and B and the output is A if A > B, otherwise the output is B.

![max2](lab07-max2.png)

Build a new circuit called Max3 that determines the maximum value of 3 inputs: A, B, and C. You can use the library circuits use above (the comparator and multiplexor) and any additional gates you may need. Give a brief explanation in English how your new circuit works. Draw neatly. Note, you cannot implement Max3 in terms of Max2, you must use components and gates.



## Question 3 - Single Cycle Processor


## Question 4 - Pipelined Processor
