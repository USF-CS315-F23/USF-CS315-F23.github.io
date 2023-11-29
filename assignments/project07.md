---
layout: default
title: Project07
nav_order: 13
parent: Assignments
permalink: /assignments/project07
---

# RISC-V Pipelined Processor Hazard Unit

## Circuits due Wed Dec 6th by 11:59pm in your Project07 GitHub repo

## There is no interactive grading for Project07

## Deliverables

1. For this project, you will start with a given, basic implementation of a pipelined processor. This is a 5-stage pipeline processor that includes pipeline registers between each stage. To execute code on this processor, code must contain `nop` (no operation) instructions (`addi x0, x0, 0`) between real instructions so that the register file is updated properly. We will learn how this processor works in class this week.

1. You will evolve the given implementation to include data path additions and a Hazard Unit, which provide support for register forwarding for data dependencies, stalling for memory reads, and flushing the pipeline to handle branches. The ultimate goal is to remove the need for `nop` instructions to properly execute code and to allow the pipeline to run at the fastest rate possible.

1. You will commit the entire implementation of your pipelined processor, including the given circuits and ROMs, to your assignment repo. Your top-level processor must be named project07.dig.

## Requirements

1. For this project you will evolve the given processor design to include data path additions and a Hazard Unit. The Hazard Unit gives the processor the ability to automatically handle the hazard conditions described in the following test cases.

**04-add-fwd.s**

```
main:
    li a1, 1
    li a2, 2
    add a0, a1, a2   # a0 should be 3
    unimp            # marker instruction
```
This program illustrates a Data Hazard, since `a0` depends on the Writeback stage of the two immediate-form `li` (`addi`) instructions. Your Hazard Unit will enable Register Forwarding so that the values of `a1` and `a2` are available when the `add` instruction begins the Execute stage.

**05-ld-stl.s**

```
main:
    li a0, 0
    li a1, 1
    sw a1, (a0)
    lw a2, (a0)
    addi a0, a2, 1   # a0 should be 2
    unimp            # marker instruction
```
This program illustrates the need to Stall the pipeline, since the addi instruction depends on the value loaded by the `lw` instruction. Your Hazard Unit will enable a Stall in the appropriate Pipeline Registers.

**06-jal-fls.s**

```
main:
    li a0, 2
    jal foo
    unimp           # marker instruction
foo:
    addi a0, a0, 4  # a0 should be 6
    ret
```
This program illustrates a Control Hazard. Since the `jal` instruction means that subsequent instructions (the marker) should not be executed, we need to Flush the pipeline.

## Given

1. You may use any of the circuits shown in lecture, including the pipeline registers and disassembly circuit.

1. The project07 starter circuits includes a simplified pipelined processor including:
    1. Support for RAM including lw and sw
    1. An implementation of the ASCII-based disassembler, including the decoder which outputs the inum to match the disassembler's ROM.
    1. Instruction memory, including assembly source and hex files, for:
        1. Four programs which execute using nop instructions to manually avoid hazards
        1. The three programs which require your Hazard Unit to execute correctly

1. You may use any of Digital's built-in components, or your own if you prefer.

## Rubric
- 10 pts: passes `00-add-3nop`, `01-add-2nop`, `02-jal`, `03-lw` test cases
- 70 pts: passes the `04-add-fwd` test case
- 10 pts: passes the `05-ld-stl` test case 
- 10 pts: passes the `06-jal-fls` test case

