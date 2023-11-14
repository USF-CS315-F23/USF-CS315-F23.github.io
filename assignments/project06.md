---
layout: default
title: Project06
nav_order: 12
parent: Assignments
permalink: /assignments/project06
---

# RISC-V Processor Implementation

## Circuits due Mon Nov 20th by 11:59pm in your Project06 GitHub repo

## Interactive Grading on Tue Nov 21st and Wed Nov 22nd.

## Deliverables

1. Submit all of the .dig files and .hex files required to run your processor implementation
1. Submit a PDF for your Decoder spreadsheet

## Requirements

1. You will implement a single-cycle microarchitecture for a subset of the RISC-V instruction set architecture in Digital
    1. You may use any of Digital's library of components
    1. We will introduce some new and useful tools and techniques for Digital in lecture
1. You need to pass unit tests and complete program tests provided below. Note that the complete program requirements are the same as for Project04. The unit tests will help you incrementally build and test your processor. We will provide a starter instruction memory with the unit tests and the complete program tests from Project04.
1. In order to make your own programs for testing runnable on your processor you must do the following:
    1. You will add an assembly language main to set up at least five parameters for your functions
    1. You will add the end marker (`unimp`) to indicate when the program should stop
    1. Ensure that you do not have `.global` directives in your programs
1. Your processor implementation will include the following major sub-circuits:
    1. The Program Counter (`PC`) will be one 64-bit register with enable (`EN`) and clear (`CLR`) inputs.
    1. Machine code programs will be stored in ROM components, just as we did in Project05. Like in Project05 your instruction memory will be able to select the program you want to execute.
    1. A Register File that can support reading 2 registers in a single clock cycle: `RD0`, `RD1`
    1. The Arithmetic Logic Unit (ALU) will perform data processing tasks such as `add`, `sub`, `mul`, and shifting (`sll`, `slr`, `sra`). You will need to support more than these operations.
    1. The Branch Control Unit (BCU) will perform conditional branch computation and PC updates. You will need to decide where to put this logic in the top-level circuit.
    1. Data Memory. We will use a Digital RAM component for data memory. This will be used for stack memory by our test programs. We will configure the RAM component to hold 64 bit word values. This will make it easy to support `ld` and `sd`. You will need to add logic for word- and byte-size memory operations.
    1. The Control Unit will decode machine code instructions and generate control signals.
    1. The Data Path will connect data between the various sub-circuits
    1. The Control Path will connect the Control unit to various sub-circuits and multiplexers
    1. Your top-level processor circuit should have outputs for registers A0, A1, A2, and A3.

## Given
1. We will discuss the major sub-circuits in lecture and you will have hands-on time to develop and ask questions
1. We have compiled [Processor Guide Part 3](/guides/processor-part-3.html)
1. We will provide a starter instruction memory with the unit tests and complete program tests.
1. We will provide a Python script for creating a decode ROM hex file.

## Tests

See the tests repo for Project06. You will have to copy the inst-mem.dig file from the Projec06 test repo to your project06 repo.

## Rubric

For interactive grading you should be able to run the autograder tests and manually execute any required test.
- (30 points) Automated unit tests
- (50 points) Automated prog tests
- (5 points) Interactive grading question #1
- (5 points) Interactive grading question #2
- (10 points) Neatness
