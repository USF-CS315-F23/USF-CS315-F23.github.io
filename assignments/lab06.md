---
layout: default
title: Lab06
nav_order: 11
parent: Assignments
permalink: /assignments/lab06
---

# Processor Design Part 2

## Circuits due Mon Nov 13th by 11:59pm in your Lab06 GitHub Repo

## Requirements

1. For this lab, please use an incremental development approach and commit two top-level circuits: `lab06-part1.dig` and `lab06-part2.dig`. 
1. Each top-level circuit should contain its own decoder circuit to identify instructions and propagate the appropriate control signals. 
1. For this lab you will combine your work from lab05 with a new implementation of the control lines as described in the [Processor Guide Part 2](/guides/processor-part-2.html)
1. Use the spreadsheet approach to develop a decoder table that associated inputs (opcode, funct3, funct7, and funct6) with decoder outputs (`RFW`, `ALUOp`, etc.).
1. You must have inputs for `CLK`, `EN`, `CLR`, and `PROG`, and outputs for `A0`, `A1`, `A2` and `DONE`
1. Submit all your `.dig` files, `.s` and `.hex` files, and a PDF of your instruction decoder spreadsheet. 

## Part 1

1. Build decoders (for instructions, registers, and immediates) and top-level processor circuit which can execute this program (also given in the Guide)

    ```
    first_s:
        li a0, 1
        li a1, 2
        add a2, a0, a1
        unimp
    ```
## Part 2

1. Build the control and top-level processor circuit which can execute this program (also given in the Guide)
    ```
    main:    
        li a0, 1
        li a1, 2
        jal first_s
        unimp
    first_s:
        add a0, a0, a1
        ret
    ```
## Given

1. You may use any of the circuits shown in the [Processor Guide Part 2](/guides/processor-part-2.html)
1. You may use any of Digital's built-in components, or your own if you prefer.

## Rubric

100 pts: 50 pts for each part, as shown by the autograder
