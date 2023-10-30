# Key Concepts from Labs and Projects

## Lab01 - Hello World in the RISC-V Dev Env

- Console based editing: micro or vim
- git and GitHub usage
- Makefiles
- Hello World in C
- The Autograder

## Project01 - RISC-V VM Access and Number Conversion in C

- Dev Environment Setup
  - Local RISC-V VM using qemu-system-riscv64
  - Remote RISC-V VM on euryale
  - ssh keys, ssh `config`, ssh `authorized_keys`
  - Shell configuration
    - bash: `~/.profile`, `~/.bash_profile`, `~/.bashrc`, `~/.bash_aliases`
    - zsh: `~/.zprofile`, `~/.zshrc`
- C Basics
  - Functions
  - Data (global, stack, heap)
  - Statements and expressions
  - Data types
    - Primitives - `int, `char`, `float`, `uint8_t`, `uint32_t`, `int32_t`, `uint64_t`, `int64_t`
    - Composit - arrays and structs
    - Pointers - `int *`, `char *`, `uint32_t *`, `uint64_t *`
  - Pointer operations
    - & address of, e.g., `int x; int *p; p = &x;`
    - * dereference, e.g., `x = *p;`
  - Strings in C
    - Array of chars (bytes)
    - Null `\0` (0) terminated
    - ASCII character codes
  - The C stack
    - Local variables
    - Allocated on function entry, deallocated on function exit (return)
  - Signed (`int`, `int32_t`) vs unsigned (`unsigned int`, `uint32_t`) values
- Command line arguments with `int argc` and `char *argv`
  - Layout of `argv` array in memory
  - Array of pointers first, `argv[0]`, `argv[1]`, `argv[2]`, `NULL (0)`
  - String arrays second, `./foo` + `\0`, `-p` + `\0`, etc.
- Parsing command line arguments
- Number systems
  - Decimal (base 10, digits: `0`, `1`, `2`, `3`, `4`, `5`, `6`, `7`, `8`, `9`)
  - Binary (base 2, digits: `0`, `1`, C literal prefix: `0b`)
  - Hexadecimal, "hex" (base 16: digits: `0`, `1`, `2`, `3`, `4`, `5`, `6`, `7`, `8`, `9`, `A`, `B`, `C`, `D`, `E`, `F`, C literal prefix: `0x`)
    - `A` = 10, `B` = 11, `C` = 12, `D` = `13`, E = `14`, F = `15`
- base conversions
  - C string as decimal, binay, hexadecimal to int
    - Place value: `2 * (10^2) + 5 * (10^1) + 4 * (10^0)`   
  - int to C string as decimal, binary, hexadecimal
    - Modulus to find digit
    - Divide to get value for next digit
    - Repeat until divide gives 0   

## Lab02 - Introduction to RISC-V Assembly Programming

- Instructions, registers, labels, directives
  - See [RISC-V Assembly Guide]({{ site.url }}/guides/riscv-asm.html)
- Assembling vs compiling
  - Assembling (as) assembly source (.s) to object code (.o)
  - Compiling (gcc) c source (.c) to object code (.o)
  - The object files must be linked, we can use gcc for this
    - `as -o foo.o foo.s`
    - `gcc -o main main.c foo.o`      
  - The C compiler can also assemble
    - `gcc -o main main.c main.s`
- Instructions have a name (mnemonic), like `add` or `mul`
- Assembly programming model
  - A RISC-V processor has 32 registers plus the `PC` registers
    - `x0`, `x1`, ... `x31`
    - ABI names: `sp`, `ra`, `a0`, `a1`, ..., `t0`, `t1`, ..., `s0`, `s1`, ...
  - The `PC` points to next instruction in memory to execute
  - Load an instruction from memory into processor
  - Execute the instruction, usually updates one or more register values, but can also update memory
  - Update `PC`, often just the next instruction `PC + 4`, but can also be a diffenent address in the case of a branch or jump (control instruction)
- Most instructions have three operands
  - One target (destination)
  - Two source
  - `add t0, t1, t1` means `t0 = t1 + t2`
- On RISC-V 64 bit registers are 64 bits (8 bytes)
- Data processing instructions
- Immediate values
- Control instructions
  - conditional branchs: `beq`, `bne`, `blt`, `bge`
  - jumps: `j`
  - return: `ret`
- Assembly arguments passed in `a0`, `a1`, `a2`, `a3`, etc.
- Return value is put into `a0`
- Basic assembly function
  ```text
  .global func_name
  func_name:
      add a0, a0, a1
      ret
  ```
- if/then/else in assembly
  - C:
  ```text
  int x, y, r;
  r = 0;
  if (x > y) {
     r = r + 1;
  } else {
     r = r + 99;
  }
  ```
  - Asm: Assume `a0 - int x, a1 - int y, t0 - int r`
  ```
      li t0, 0
      bgt a0, a1, else
      addi t0, t0, 1
      j endif
  else:
      addi t0, t0, 99
  endif:
  ```
- for loops in assembly
  - C:
  ```
  int r, i, n;
  r = 0;
  n = 10
  for (i = 0; i < n; i++) {
      r = r + i;
  }
  ```
  - Assume `t0 - int r, t1 - int i, t2 - int n`
  ```
      li r, 0
      li n, 10
      li i, 0
  loop:
      bge t1, t2, loopend
      addi t0, t0, t1
      addi t1, t1, 1
      j loop
  loopend:
  ```
- Array access in assembly
  - lw (load word) `lw t0, (a0)` means `t0 = *a0`
  - Load word at address `a0` into register `t0`
  - Pointer based array access
    - To get to next element in array of words (ints): `addi a0, a0, 4`

## Project02 - RISC-V Assembly Language

- RISC-V ABI Function Calling Conventions
  - Only one set of registers (32), so we need a way to coordinate usage between functions
  - Caller-saved registers (`a0`, `a1`, ..., `a7`, `t0`, `t`, ... `t6`)
    - These must be preserved on the stack by the caller of a function
    - You only need to preserve the registers you will be using after the function call
  - Callee-saved registers (`sp`, `ra`, `s0`, `s1`, ... `s11`)
    - These must be preserved on the stack by the callee, on entry to the function.
    - They should be restored on exit.
    - The stack pointer is preserved by subtracting (stack allocation) on function entry and adding (stack deallocation) on function exit the name number of bytes.
    - The `sp` should be a multiple of 16 (for performance compatibility with some instructions)
- Functions that don't call other functions do not need to allocate stack space.
  - Only need stack space if the function uses caller-saved registers
- Function call template when using stack:
  ```text
  .global func_name
  func_name:
      addi sp, sp, -N    # Allocate N bytes on stack, N must be a multiple of 16
      sd ra, (sp)         # Save ra onto stack
                          # Save any callee-saved registers if needed
      ...
      ld ra, (sp)
      addi sp, sp, +N
      ret
  ```
- Indexed-based array access
  - Assume `t0` is `int i`, `a0` is base address of array `int arr[]`
  ```text
  li t1, 4
  mul t1, t1, t0
  add t1, a0, t1
  lw t2, (t1)
  ```
  - Altneratively use a shift instead of multiply
  ```text
  slli t1, t0, 2
  add t1, a0, t1
  lw t2, (t1)
  ```
- Recursive functions
  - Nothing special, just that the func calls itself
  - Same rules apply
  - Presere any caller-saved registers on stack before the recursive call
  - Restore any caller-saved registers from stack after the recursive call
  - If a recursive function simply returns after the recursive call, then it is called *tail recursive* and the recursive call can be turned into a jump.
 
 
## Project03 - RISC-V Assembly Language Part 2

- Working with C strings in assembly
- Use `lb` (load byte) and `sb` (store byte) to access characters in a string
- Calling C functions from Assembly
  - Just add a `.global func_name` for the function you want to use
  - E.g., `.global strlen`
  - Then set arg registers and use `call` instruction as normal
  - Make sure to preserve any caller-saved registers before call 
- Memory
  - Byte addressable
  - Smallest addressable unit is a byte (8 bits)
  - A int (word) is 4 bytes, need to decide how to layout a word in memory
  - Two byte orderings
    - Big endian (put the most significant byte first)
    - Little endian (put the least significant byte first)
    ```text
    int x = 0xFFAA1122
    4 |    |   4 |    |
    3 | 22 |   3 | FF |
    2 | 11 |   2 | AA |
    1 | AA |   1 | 11 |
    0 | FF |   0 | 22 |
    Big        Little
    Endian     Endian
    ```
- Two's Complement (2's Complement)
  - Need a way to represent negative integer values
  - To get the two's complement negative representation of a positive value:
    - twos = invert_bits(val) + 1
  - 3-bit two's complement values
  ```text
  Bin Dec
  000   0
  001   1
  010   2
  011   3
  100  -4
  101  -3
  110  -2
  111  -1
  ```
  - Two's complement properties
    - The msb (most significant bit) is the sign bit (0 is pos, 1 is neg)
    - Only one representation of 0
    - Because of this, there is one more neg value than pos values
    - Simple grade school arithmetic work on two's complement numbers
- Bit manipulation and bitwise operators
  - Zero (0) / One (1)
    - Other names: unset/set, low/higg, off/on, false/true
  - A byte is 8 bits
    ```text
    msb | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 | lsb
    ```
    - msb - most significant bit
    - lsb - least significant bit
  - A word is 4 bytes or 32 bits
    ```text
    msb | 31      24|23      16|15       8|7        0| lsb
        |   byte 3  |  byte 2  |  byte 1  |  byte 0  |
    ```
  - Bitwise Operaters (op, C, ASM)
     - `AND` ,  `&`  , `and/andi`
     - `OR`  ,  `|`  , `or/ori`
     - `NOT` ,  `~`
     - `XOR` ,  `^`  , 
     - Shift Left Logical
       - Shift x to the left by n bits
       - Fill lower bits with 0
       - `uint32_t x`
       - `x << n`
       - `sll/slli`
     - Shift Right Logical
       - Shift x to the right by n bits
       - Fill upper bits with 0
       - `uint32_t x`
       - `x >> n`
       - `srl/srli`
     - Shift Right Arithmetic
       - Shift x to the right by n bits
       - Fill upper bits with sign bit value
       - `int32_t x`
       - `x >> n`
       - `sra/srai`
  - Masking
    - It is often need to pull out a sequence of bits from a larger value
      - To get a sequence of bits:
      - Shift the bits to the far right
      - Mask the bits to zero out any upper bits we don't want
      - Example: assume we want the middle 4 bits of an 8 bit value (bits 2-5)
      ```text
      uint8_t x = 0b11011010;
      uint32_t v = x >> 2     // Shift the 4 bits to the beginning
      uint32_t v = v & 0b1111 // Mask off upper bits remaining in v
      ```
   - Combine individual bit sequences with right shift and OR (`|`)
   - Sign extension
     - Assume you have a 4-bit two's complement number you want to sign extend to be a 32-bit two's complement number.
     ```text
     int32_t v = 0b1110 // -2 in 4 bits
     v = (v << 28) >> 28;
     ```
     - Shift all the way to the left and then do shift right arithmetic all the way back

## Lab03 - RISC-V Machine Code Emulation

- RISC-V Machine Code and Emulation
  - Instructions are 32 bits (1 word), there is also a 16 bit compressed format
  - Each instruction encodes source register(s), a destination registers, and target address or offset
  - We  decode the `iw` (instruction word) in steps
      - Look at `opcode` to determine instruction format type
      - Decode the `iw` based on opcode
      - Further decode `iw` by looking at other fields such as `funct3` and `funct7`
  - The base RISC-V emulator includes the emulated state as a struct:
  ```text
  struct rv_state {
      uint64_t regs[NREGS];
      uint64_t pc;
      uint8_t stack[STACK_SIZE];
  };
  ```
    - `NREGS` is 32 (because RISC-V has 32 registers)
    - The `pc` is the program counter
    - `STACK_SIZE` is 8192, but for prorgrams with lots of function calls and/or local vars this may need to be bigger.
  - Basic emulation
    - We will emulate assembly programs that have been linked with the emulator code
    - Initialize the `rv_state` with
      - A pointer to the function we want to emulate
      - The first 4 arguments as uint64_t values to send to the function
    - Get `iw`: `iw = *pc`
    - Get `opcode` from `iw`
    - Based on `opcode` call emulation function, like:
      - `emu_r_type(rsp, iw)`
      - `rsp` is `struct rv_state *rsp`
    - The emu functions will further decode the `iw`
      - Use shift and mask (`get_bits()`) to get each field
      - Update the state (registers and memory based on `opcode`, `funct3`, and `funct7`)
      - Update the `pc`, usually `pc += 4` to go to the next instruction
        - For control instructions, we will compute either an `offset` and add to `pc`
        - Or, for `jalr` use a register value to update the `pc`
- RISC-V Instruction Formats
  - R-type `|funct7[31:25]|rs2[24:20]|rs1[19:15]|funct3[14:12]|rd[11:7]|opcode[6:0]|`
    - For register instructions like `add`, `sub`, `sll`
  - I-type `|imm_11_0[31:20]|rs1[19:15]|funct3[14:12]|rd[11:7]|opcode[6:0]|`
    - For immediate instruction like `addi`, `slli`, `lw`, `lb`
    - Immediate must be constructed from parts and sign extended
    - `int64_t imm = sign_extend(imm_11_0, 11)`
  - B-type `|imm_12[31]|imm_10_5[30:25]|rs2[24:20]|rs1[19:15]|funct3[14:12]|imm_4_1[11:8]|imm_11[7]|opcode[6:0]|`
    - For branches, like `beq`, `bne`, `blt`, `bge`
    - `int64_t imm = sign_extend(imm_12 << 12 | imm_11 << 11 | imm_10_5 << 5 | imm_4_1 << 1)`
    - If branch taken `pc += imm`, if not taken `pc += 4`
  - J-type `|imm_20[31]|imm_10_1[30:21]|imm_11[20]|imm_19_12[19:12]|rd[11:7]|opcode|`
    - For 'jal' jump and link, also 'j'
    - `int64_t imm = sign_extend(imm_20 << 20 | imm_19_12 << 12 | imm_11 < 11 | imm_10_1 << 1)`
    - `pc += imm`
- RISC-V Pseudo Instructions
  - The assembler provide many convenience instructions that do not exist as real instructions
  - Examples
    - `li a0, 99` is `addi a0, zero, 99`
    - `j offset` is `jal zero, offset`
    - `call offset` is `jal ra, offset`
    - `bgt r1, r2, offset` is `blt r2, r1, offset`

## Project04 - RISC-V Emulation - Analysis - Cache Simulation

- Additional RISC-V Instruction Formats
  - S-type `|imm_11_5[31:25]|rs2[24:20]|rs1[19:15]|funct3[14:12]|imm_4_0[11:7]|opcode[6:0]|`
    - For store instuctions, like `sb`, `sw`, `sd`
    - Immediate must be constructed from parts and sign extended
    - `int64_t imm = sign_extend(imm_11_5 << 5 | imm_4_0, 11)`
  - U-type `|imm_31_12[31:12]|rd[11:7]|opcode[6:0]|`
    - For `lui` (load upper immediate) and `auipc` add upper immediate to pc
    - Our code dosn't use these
- Note that load instructions are an I-type form
- Dynamic Analysis
  - Because we emulate each instruction we can do the following
    - Count the total number of instructions executed
    - Count the different types of instructions executed
      - I/R count, Loads, Stores, Jumps, Branches Taken, Branches Not Taken
    - Counts added to `struct rv_state` as a new struct `struct rv_analysis`
    - Need to update counts accurately in the emulation functions
- Cache Memory and Simulation
  - A Cache is a small and fast memory that holds recently accessed data from main memory
  - Main memory is slow relative to CPU speed
  - A cache can work because program exeuction adheres to two principles of locality
    - Principle of spatial locality - If code accesses a value, there is good chance it will access nearby values
    - Principle of temporal locality - If code accesses a value now there is a good chance it will do so again soon
  - We send an address to a cache an request a value back
    - Hit - value is in the cache for the requested address
    - Miss - value is not in the cache for the requested address
    - Hit Rate = #hits / #refs
    - Miss Rate = #misses / #refs
    - In modern processes caches can achieve very high hit rates, > 95%
  - Main cache ideas
    - For a given memory address, how to locate value in the cache?
    - How to replace a value?
    - How many bytes do we load at at time into the cache (block size)?
  - Three main cache designs
    - Direct Mapped
    - Fully Associative
    - Set Associative
  - Direct Mapped
    - Each address maps to one slot in the cache
    - On miss, just repace slot value with new value from memory
  - Fully Associative
    - Each address can map to any slot in the cache
    - Use a form of LRU (least recently used) to choose which slot to evict on miss
  - Set Associative
    - An address maps to a set of slots, and can map to any slot in the set
    - Use LRU within the slots in a set to choose a slot to evict
  - Block Size
    - To support spatial locatlity a cache will read in more than the word requested
    - We can think of memory as an array of blocks
    - We need to find the block for a give address
    - Find the base of the block
    - The load the entire block into the cache slot
