# RV32I 5-Stage Pipelined RISC-V Processor with Spike and RISCV-DV Verification

## Project Overview

This project implements a **32-bit RISC-V (RV32I) five-stage pipelined
processor** in **SystemVerilog**, together with a verification
environment based on **Spike**, the official RISC-V Instruction Set
Simulator (ISS), and **RISCV-DV**, the open-source constrained-random
instruction generator developed by CHIPS Alliance.

The processor is developed and simulated using **Vivado**, while
software programs are compiled using the **RISC-V GCC Toolchain**.
Generated executables are executed on both the RTL and Spike to ensure
architectural correctness.

## Project Objectives

-   Design a complete RV32I processor implementing the base integer
    instruction set.
-   Implement a classic 5-stage instruction pipeline.
-   Support hazard detection, forwarding, and pipeline flushing.
-   Execute assembly and C programs compiled using the RISC-V GNU
    toolchain.
-   Verify functional correctness using Spike.
-   Automate large-scale testing using RISCV-DV.
-   Compare RTL execution against a golden reference model.

## Processor Architecture

``` text
Instruction Memory
        │
        ▼
IF : Instruction Fetch
        │
        ▼
ID : Instruction Decode
        │
        ▼
EX : Execute / ALU
        │
        ▼
MEM : Data Memory
        │
        ▼
WB : Write Back
```

## Pipeline Stages

### IF -- Instruction Fetch

-   Program Counter generation
-   PC + 4 calculation
-   Instruction memory access
-   Branch/jump target selection

### ID -- Instruction Decode

-   Register file read
-   Immediate generation
-   Control decoding
-   ALU control decoding

### EX -- Execute

-   ALU operations
-   Branch comparison
-   Forwarding
-   Branch target calculation

Supported ALU operations: - ADD - SUB - AND - OR - XOR - SLL - SRL -
SRA - SLT - SLTU

### MEM -- Memory Access

-   Load instructions
-   Store instructions
-   Data memory interface

### WB -- Write Back

-   Write ALU results
-   Write load data
-   Write PC + 4

## Supported ISA

### Arithmetic

ADD, ADDI, SUB, SLT, SLTU, SLTI, SLTIU

### Logical

AND, ANDI, OR, ORI, XOR, XORI

### Shift

SLL, SLLI, SRL, SRLI, SRA, SRAI

### Memory

LB, LH, LW, LBU, LHU, SB, SH, SW

### Branch

BEQ, BNE, BLT, BGE, BLTU, BGEU

### Jump

JAL, JALR

### Upper Immediate

LUI, AUIPC

## Pipeline Registers

-   IF/ID
-   ID/EX
-   EX/MEM
-   MEM/WB

## Hazard Handling

### Data Hazards

-   EX/MEM forwarding
-   MEM/WB forwarding

### Load-Use Hazard

-   Stall IF and ID stages
-   Flush ID/EX control signals

### Control Hazards

-   Branch resolution in EX stage
-   Pipeline flush on taken branch/jump

## Register File

-   32 general-purpose registers
-   Two asynchronous read ports
-   One synchronous write port
-   x0 permanently tied to zero

## Immediate Generator

Supports: - I-Type - S-Type - B-Type - U-Type - J-Type

## Development Tools

-   SystemVerilog
-   Vivado
-   Spike
-   RISCV-DV
-   RISC-V GCC Toolchain
-   Ubuntu Linux

## Verification Flow

``` text
Assembly / C
      │
      ▼
RISC-V GCC
      │
      ▼
ELF
 ├────────► Spike
 │             │
 ▼             ▼
objcopy   Reference Trace
 │
 ▼
HEX
 │
 ▼
RTL Simulation
 │
 ▼
RTL Trace
 │
 ▼
Comparison
```

Spike serves as the architectural reference model. Every retired
instruction from the RTL is compared against Spike.

## RISCV-DV Verification

RISCV-DV is used to: - Generate constrained-random instruction streams -
Compile generated programs - Execute them on Spike - Produce ELF/BIN/HEX
outputs - Execute the same programs on RTL - Compare RTL against the
reference model

## Project Directory Structure

``` text
riscv_core_project/
├── rtl/
├── tb/
├── firmware/
├── tools/
├── generated/
└── docs/
```

## Testing Methodology

### Stage 1

Directed instruction tests.

### Stage 2

Execution of complete assembly and C programs.

### Stage 3

Large-scale constrained-random verification using RISCV-DV.

## Future Enhancements

-   RV32M extension
-   CSR support
-   Exception and interrupt handling
-   Branch prediction
-   Caches
-   AXI interface
-   UVM verification
-   FPGA implementation

## Conclusion

This project implements a complete RV32I pipelined processor together
with a verification flow based on the RISC-V GCC toolchain, Spike, and
RISCV-DV. The approach combines directed testing and constrained-random
verification to validate architectural correctness and provides a
foundation for future ISA extensions and microarchitectural
improvements.
