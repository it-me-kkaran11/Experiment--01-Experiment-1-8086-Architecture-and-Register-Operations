# Experiment--02-Experiment-1-8086-Architecture-and-Register-Operations

## Aim

To study the architecture of the 8086 microprocessor and write assembly
language programs for register-to-register data transfer, memory
storage, and exchanging two 16-bit memory values.

## Learning Outcomes

After completing this experiment, students will be able to: - Explain
the main functional units of the 8086 microprocessor. - Describe the
roles of the Bus Interface Unit (BIU) and Execution Unit (EU). - Write
8086 assembly instructions for 16-bit data transfer. - Exchange data
between two memory locations using registers.

## Software Required

-   An 8086 assembly emulator, such as emu8086 or an equivalent
    8086-compatible emulator.
-   A text editor for writing the assembly programs.

------------------------------------------------------------------------

## Part (a): 8086 Architecture and Functional Units

### Question

Explain the architecture of the 8086 microprocessor with a neat block
diagram, describing the functions of the BIU and EU.

### Theory

The Intel 8086 is a 16-bit microprocessor with a 16-bit data bus and a
20-bit address bus. It can address up to 1 MB of physical memory. Its
architecture is divided into two major units:

**1. Bus Interface Unit (BIU)** - Generates the 20-bit physical
address. - Fetches instructions from memory. - Reads data from and
writes data to memory and I/O devices. - Contains the segment registers:
CS, DS, SS, and ES. - Contains the Instruction Pointer (IP). - Includes
a 6-byte instruction prefetch queue, allowing instruction fetching and
execution to overlap.

**2. Execution Unit (EU)** - Decodes and executes instructions fetched
by the BIU. - Contains the Arithmetic Logic Unit (ALU). - Contains
general-purpose registers AX, BX, CX, and DX, which can be accessed as
8-bit halves. - Contains pointer and index registers SP, BP, SI, and
DI. - Contains the Flag Register, which records status and control
flags. - Requests the BIU to perform memory and I/O transfers when
required.

### Simplified Block Diagram

``` text
                 +-----------------------------------+
                 |          8086 MICROPROCESSOR     |
                 |                                   |
Memory / I/O <-->+-----------------------------------+<--> System Bus
                 |       Bus Interface Unit (BIU)    |
                 |  +-----------------------------+  |
                 |  | Segment Registers            |  |
                 |  | CS, DS, SS, ES               |  |
                 |  +-----------------------------+  |
                 |  | Instruction Pointer (IP)     |  |
                 |  +-----------------------------+  |
                 |  | 6-byte Instruction Queue     |  |
                 |  +-----------------------------+  |
                 |  | Address Generation / Bus     |  |
                 |  | Control                      |  |
                 |  +---------------+-------------+  |
                 |                  |                |
                 |       Instruction/Data Interface  |
                 |                  |                |
                 |       Execution Unit (EU)         |
                 |  +-----------------------------+  |
                 |  | Instruction Decoder &       |  |
                 |  | Control Circuitry           |  |
                 |  +-----------------------------+  |
                 |  | ALU                         |  |
                 |  +-----------------------------+  |
                 |  | AX, BX, CX, DX               |  |
                 |  | SP, BP, SI, DI               |  |
                 |  | Flag Register                |  |
                 |  +-----------------------------+  |
                 +-----------------------------------+
```

### Working Principle

The BIU fetches instruction bytes and stores them in its 6-byte queue
while the EU executes instructions already available in the queue. When
the EU needs memory or I/O access, it requests the BIU to perform the
bus operation. A change in program flow, such as a jump, may cause the
instruction queue to be flushed and refilled.

------------------------------------------------------------------------

## Part (b): Transfer a 16-bit Value Between Registers and Store It in Memory

### Question

Write an 8086 assembly language program to transfer a 16-bit data value
from one register to another and store the result in memory.

### Algorithm

1.  Load a 16-bit value into AX.
2.  Transfer the value from AX to BX.
3.  Store the value in BX into the memory variable `RESULT`.
4.  Stop the program.

### Assembly Program

``` asm
.MODEL SMALL
.STACK 100H

.DATA
    RESULT DW 0000H

.CODE
MAIN PROC
    MOV AX, @DATA
    MOV DS, AX

    MOV AX, 1234H       ; Load 16-bit value into AX
    MOV BX, AX          ; Copy AX into BX
    MOV RESULT, BX      ; Store BX in memory

    MOV AH, 4CH
    INT 21H
MAIN ENDP
END MAIN
```
### Expected Result

-   AX = `1234H`
-   BX = `1234H`
-   RESULT = `1234H`

### Explanation

`MOV BX, AX` copies the 16-bit contents of AX into BX without changing
AX. `MOV RESULT, BX` stores the contents of BX in the 16-bit memory
variable `RESULT`. The `MOV` instruction does not modify the source
operand.
------------------------------------------------------------------------

## Part (c): Exchange Two 16-bit Memory Locations Using Registers

### Question

Write an 8086 assembly language program to exchange the contents of two
16-bit memory locations using registers.

### Algorithm

1.  Load the first memory value into AX.
2.  Load the second memory value into BX.
3.  Store BX at the first memory location.
4.  Store AX at the second memory location.
5.  Stop the program.

### Assembly Program
.MODEL SMALL
.STACK 100H

.DATA
    VALUE1 DW 1111H          ; First 16-bit memory location
    VALUE2 DW 2222H          ; Second 16-bit memory location

.CODE
START:
    ; Initialize Data Segment
    MOV AX, @DATA
    MOV DS, AX

    ; Step 2: Load the first memory value into AX
    MOV AX, VALUE1           ; AX = 1111H

    ; Step 3: Load the second memory value into BX
    MOV BX, VALUE2           ; BX = 2222H

    ; Step 4: Store BX at the first memory location
    MOV VALUE1, BX           ; VALUE1 = 2222H

    ; Step 5: Store AX at the second memory location
    MOV VALUE2, AX           ; VALUE2 = 1111H

    ; Step 6: Stop the program / Return to DOS
    MOV AH, 4CH
    INT 21H
END START

### Expected Result

  Memory Variable     Before Execution   After Execution
  ----------------- ------------------ -----------------
  VALUE1                       `1111H`           `2222H`
  VALUE2                       `2222H`           `1111H`

### Explanation

  • .DATA: Tells the computer to allocate space in the RAM. We create two 16-bit boxes (DW stands for Define Word, which means 16 bits): VALUE1 holding 1111H and VALUE2 holding 2222H.
• MOV AX, @DATA & MOV DS, AX: The CPU needs to know where in the RAM your variables are located. This initializes the Data Segment (DS) register so the program can find VALUE1 and VALUE2.

------------------------------------------------------------------------

## Procedure

1.  Open an 8086-compatible assembler or emulator.
2.  Create a new assembly source file.
3.  Enter and assemble the program for the selected part.
4.  Correct any syntax or assembler-specific errors.
5.  Execute the program step by step or run it.
6.  Inspect the registers and memory variables.
7.  Record the observed results and compare them with the expected
    results.
## Screenshot of the results 
<img width="1747" height="855" alt="Screenshot 2026-10-27 162632" src="https://github.com/user-attachments/assets/980c5f00-fc2b-4e00-900c-120ccb6eeb01" />
<img width="1735" height="807" alt="Screenshot 2026-10-27 163703" src="https://github.com/user-attachments/assets/b1448c30-042d-43dc-8ca9-843483797c1e" />

## Result

The 8086 architecture and the functions of the BIU and EU were studied.
The assembly programs demonstrate 16-bit register transfer with memory
storage and the exchange of two 16-bit memory values.

## Viva Voce Questions

1.  What is the function of the BIU in the 8086?
2.  What is the role of the EU?
3.  Why does the 8086 use a 6-byte instruction queue?
4.  What is the maximum physical memory address space of the 8086?
5.  What is the difference between a register and a memory variable?
6.  Does the `MOV` instruction change the source operand?
7.  Why are registers used when exchanging two memory values?
8.  What is the purpose of the DS segment register?
