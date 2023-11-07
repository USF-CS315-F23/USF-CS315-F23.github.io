---
layout: default
title: Lab05
nav_order: 10
parent: Assignments
permalink: /assignments/lab05
---

# Processor Design Part 1

## Circuits due Mon Nov 6th by 11:59pm in your Lab05 GitHub Repo

## Requirements
For this lab you should implement all the components and the top-level partial processor circuit described in the [Processor Guide Part 1](/guides/processor-part-1.html) with the following exceptions: the Extender and Data Memory for this lab.
In summary you need to implement:
1. A Program Counter using  a 64-bit Register with CLR. You may use your own register, or the Digital Register component
1. A Register File that has 32 registers. Two registers can be read, and one can be written, in a single clock cycle.
1. An ALU that supports `addi`, `sub`, `mul`, `sll`, and `srl`.
1. Your top-level processor should have a variation of the dashboard view using splitters, tunnels, and probes as shown in the guide. You do not have the replicate this view identically, but it should show the same information. You are free to come up with new and better ways to display the same information.
1. Your top-level circuit should be able to increment through a program in instruction memory showing each instruction word for the specified program, although this is not tested by the autograder in this lab.
1. By manipulating the inputs to the Register File, ALU, `CLK`, and `CLR`, your circuit should be able to execute four small programs:
    1. `addi t0, t0, 1` resulting in `T0` = 1 
    1. `li t1, 2 `resulting in `T1` = 2
    1. `addi t0, t0, -1` resulting in `T0` = -1 (0xFFFFFFFF)
    1. this multi-instruction program

        `li t0, 1` resulting in `T0` = 1

        `li t1, 1` resulting in `T1` = 1
        
        `sub t0, t0, t1` resulting in `T0` = 0
1. Your ALU should be able to subtract A - B and calculate the correct result
1. For the autograder to test the four cases above:
    1. Your main circuit must be named `lab05.dig`, have inputs named `CLK`, `CLR`, `RR0`, `RR1`, `WR`, `WE`, `ALUSrcB`, `ALUOp`, and `Imm`, and outputs named `T0` and `T1`
    1. Your `ALU` must be named `alu.dig`, have inputs named `A`, `B`, and `ALUOp`, and an output named `R`

## Given
You may use any of Digital's built-in components, or your own if you prefer.

## Rubric
100 points as shown by the autograder
