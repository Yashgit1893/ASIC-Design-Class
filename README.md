# Asic-design-class

<details>
<summary>LAB 1: C program to calculate sum from 1 to n (n=15) and then compiling the code with both GCC compiler</summary>

`sum1ton.c` is the file containing code to calculate the sum from 1 to n.

<p align="left">
  <img width="750" alt="1" src="https://github.com/user-attachments/assets/c03a8f66-e356-447a-815a-be940fdeec59">
</p>

Compiling the code using GCC compiler :
compiling the `sum1ton.c` with `gcc sum1ton.c` and run the executable file `./a.out`

<p align="left">
  <img width="750" alt="2" src="https://github.com/user-attachments/assets/9512912e-08f9-4a01-8950-b18ee442cfa4">
</p>

Output for sum from 1 to 15 is shown.

</details>

<details>
<summary>LAB 2: Compiling the sum1ton.c program of Lab 1 using RISC-V compiler. </summary>

Compiling the code using RISC-V compiler :

<p align="left">
  <img width="750" alt="3" src="https://github.com/user-attachments/assets/880561bb-45f1-466d-ada0-306014f6dbff">
</p>

compiling the `sum1ton.c` usung O1 optimization :
```bash
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
```
```bash
riscv64-unknown-elf-objdump -d sum1ton.o | less
```

<p align="left">
  <img width="750" alt="9" src="https://github.com/user-attachments/assets/ca1fdc9f-044e-4c64-b84a-ca40d518ba4f">
</p>

The number of instruction in the main function in 15

compiling the `sum1ton.c` using Ofast optimization :
```bash
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c`
```
```bash
riscv64-unknown-elf-objdump -d sum1ton.o | less
```

<p align="left">
  <img width="750" alt="6" src="https://github.com/user-attachments/assets/dac7aa97-b411-4c8e-b383-e54e9f5c3ebc">
</p>

Number of instruction in the main function is 12

Note : O1 optimization follows the standards but Ofast might break some programs, Ofast takes lesser number of instructions and provides better optimisation than O1

</details>



<details>
<summary>LAB 3: Compiling the sum from 1 to n(value of n in the code sum1ton.c of Lab 1 is changed to 100) C program using Spike simulator and debug the code. </summary>

compiling the `sum1ton.c` using the RISC-V compiler using the Ofast command :

```bash
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
```

<p align="left">
 <img width="750" alt="5" src="https://github.com/user-attachments/assets/a67b6868-c6b6-4182-8bc4-45d7a476a575">
</p>

The above image shows the output using both `./a.out ` and `spike pk sum1ton.o`. Both of them have same output for sum from 1 to 100.

### Debug 

Debug the code using spike command :

```bash
spike -d pk sum1ton.o
```

command for spike debugger to run till instruction 100b0
```bash
until pc 0 100b0
```
to check the value at the register a2
```bash
reg 0 a2
```
<p align="left">
  <img width="750" alt="8" src="https://github.com/user-attachments/assets/4afaa25c-43f5-4e74-9997-76b20d08897c">
</p>

The above image displays how the value of a2 register changes while manual debugging.
The instruction `lui a2, 0x1` instruction changes the a2 register's value from 0x0000000000000000 to 0x0000000000001000.
The instruction `lui a0, 0x21` shows the a0 register value 0x0000000000021000.




Futher steps shows the vlaue at register sp. we again run the instructions from 0 to 100b8.

```bash
until pc 0 100b8
```

check the value at the register sp
```bash
reg 0 sp
```

The below image shows the manual debug.
`addi sp, sp, -16` instruction decrements the stack pointer (sp) by 16, The value of sp register was 0x0000003ffffffb50, and is now updated to 0x0000003ffffffb40

<p align="left">
  <img width="750" alt="7" src="https://github.com/user-attachments/assets/238392fa-484a-42fb-ae67-50418e61fef1">
</p>

Note :

1. lui stands for Load Upper Immediate, operates on the higher 16 bits and addi (Add Immediate) performs addition between the immediate value and the contents of the source register, then places the result into the destination register.

</details>


<details>
<summary>LAB 4: RISC-V Instructions</summary>
  
# RISC-V instructions


RISC-V (Reduced Instruction Set Computer - V) is an open standard instruction set architecture (ISA) based on established principles of RISC. The RISC-V ISA has a small number of instruction formats, making it relatively simple to understand and implement. The primary instruction formats in RISC-V are **R-type, I-type, S-type, B-type, U-type,** and **J-type**. Each format is designed for different types of operations and encodes different information in the instruction's 32 bits.


### 1. **R-type (Register)**
- **Explanation:**
  1. Used for arithmetic and logical operations involving two source registers and one destination register.
  2. Allows complex operations like addition, subtraction, and bitwise operations.
  3. Commonly used in ALU (Arithmetic Logic Unit) operations.

- **Instruction format:**
  - **opcode (7 bits):** Operation code that specifies the instruction.
  - **rd (5 bits):** Destination register.
  - **funct3 (3 bits):** Function modifier for opcode; helps determine the specific operation.
  - **rs1 (5 bits):** First source register.
  - **rs2 (5 bits):** Second source register.
  - **funct7 (7 bits):** Additional function modifier; often used to extend the opcode.

### 2. **I-type (Immediate)**
- **Explanation:**
  1. Handles operations with one source register and an immediate (constant) value.
  2. Used in load instructions, arithmetic with immediate values, and system calls.
  3. Simplifies operations that require a constant operand.

- **Instruction format:**
  - **opcode (7 bits):** Operation code.
  - **rd (5 bits):** Destination register.
  - **funct3 (3 bits):** Function modifier.
  - **rs1 (5 bits):** Source register.
  - **imm[11:0] (12 bits):** Immediate value.

### 3. **S-type (Store)**
- **Explanation:**
  1. Facilitates storing data from a register into memory.
  2. Combines a base address (from a register) with an offset to calculate the memory address.
  3. Often used in conjunction with load instructions to manage memory.

- **Instruction format:**
  - **opcode (7 bits):** Operation code.
  - **imm[11:5] (7 bits):** Upper bits of the immediate value.
  - **rs2 (5 bits):** Source register (data to store).
  - **rs1 (5 bits):** Base address register.
  - **funct3 (3 bits):** Function modifier.
  - **imm[4:0] (5 bits):** Lower bits of the immediate value.

### 4. **B-type (Branch)**
- **Explanation:**
  1. Used for conditional branching based on comparisons between two registers.
  2. Helps in implementing control flow constructs like loops and conditional statements.
  3. Modifies the program counter based on a branch condition.

- **Instruction format:**
  - **opcode (7 bits):** Operation code.
  - **imm[12] (1 bit):** Immediate value (bit 12).
  - **imm[10:5] (6 bits):** Immediate value (bits 10-5).
  - **rs2 (5 bits):** Second source register.
  - **rs1 (5 bits):** First source register.
  - **funct3 (3 bits):** Function modifier.
  - **imm[4:1] (4 bits):** Immediate value (bits 4-1).
  - **imm[11] (1 bit):** Immediate value (bit 11).

### 5. **U-type (Upper Immediate)**
- **Explanation:**
  1. Used for loading a 20-bit immediate value into a register.
  2. Commonly used for creating large constants or setting up addresses.
  3. Essential for address calculation in conjunction with other instructions.

- **Instruction format:**
  - **opcode (7 bits):** Operation code.
  - **rd (5 bits):** Destination register.
  - **imm[31:12] (20 bits):** Upper 20 bits of the immediate value.

### 6. **J-type (Jump)**
- **Explanation:**
  1. Directly modifies the program counter to jump to a new instruction address.
  2. Includes instructions like `jal` for jumping and linking return addresses.
  3. Useful for function calls and handling program control flow.

- **Instruction format:**
  - **opcode (7 bits):** Operation code.
  - **rd (5 bits):** Destination register.
  - **imm[20] (1 bit):** Immediate value (bit 20).
  - **imm[10:1] (10 bits):** Immediate value (bits 10-1).
  - **imm[11] (1 bit):** Immediate value (bit 11).
  - **imm[19:12] (8 bits):** Immediate value (bits 19-12).

<p align="left">
  <img width="750" alt="1" src="https://github.com/user-attachments/assets/d5232af0-8c4e-4fe8-9aa9-7fc77967a66b">
</p>

### Explanation of the subfields :

### Immediate (`imm`)
A variable-length field embedded in instructions, representing a constant value. It's used directly as an operand or offset in various operations, such as arithmetic, memory access, and branching. The size and location differ across instruction formats, with some requiring sign extension.

### `funct7`
A 7-bit function code that specifies the operation variant within R-type instructions. It works with `opcode` and `funct3` to differentiate operations sharing the same basic function, like distinguishing between `add` and `sub`.

### `funct3`
A 3-bit field that further refines the operation specified by the `opcode`. It is used across multiple instruction formats (R-type, I-type, etc.) to indicate specific operations or conditions, such as arithmetic functions, memory operations, or branch types.

### `opcode`
A 7-bit field that identifies the general operation category (e.g., arithmetic, load/store, branch). It forms the basis for instruction decoding and works with `funct3` and `funct7` to determine the specific operation.

### Register Specifiers (`rs1`, `rs2`, `rd`)
- **`rs1` and `rs2`:** Source registers providing operands for an operation.
- **`rd`:** The destination register where the result is stored. These fields are central to operations across RISC-V formats, ensuring consistent and efficient register access.


### Instructions given

``` bash
 ADD r5, r6, r7
 SUB r7, r5, r6
 AND r6, r5, r7
 OR r8, r6, r5
 XOR r8, r5, r4
 SLT r10, r2, r4
 ADDI r12, r3, 5
 SW r3, r1, 4
 SRL r16, r11, r2
 BNE r0, r1, 20
 BEQ r0, r0, 15
 LW r13, r11, 2
 SLL r15, r11, r2
```


| Operation | 32-bit Instruction | Hexadecimal | Type |
|-----------|-------------------|-------------|------|
| ADD r5, r6, r7 | 00000000 00111 00110 000 00101 0110011 | 0x00B30333 | R |
| SUB r7, r5, r6 | 01000000 00110 00101 000 00111 0110011 | 0x40A282B3 | R |
| AND r6, r5, r7 | 00000000 00111 00101 111 00110 0110011 | 0x00F2F2B3 | R |
| OR r8, r6, r5 | 00000000 00101 00110 110 01000 0110011 | 0x00B301B3 | R |
| XOR r8, r5, r4 | 00000000 00100 00101 100 01000 0110011 | 0x00A281B3 | R |
| SLT r10, r2, r4 | 00000000 00100 00010 010 01010 0110011 | 0x004102A3 | R |
| ADDI r12, r3, 5 | 00000000 00101 00011 000 01100 0010011 | 0x00518313 | I |
| SW r3, r1, 4 | 0000000 00011 00001 010 00100 0100011 | 0x00312023 | S |
| SRL r16, r11, r2 | 00000000 00010 01011 101 10000 0110011 | 0x025C833 | R |
| BNE r0, r1, 20 | 00000000 00001 00000 001 00000 1100011 | 0x00100263 | B |
| BEQ r0, r0, 15 | 00000000 00000 00000 000 00000 1100011 | 0x00000063 | B |
| LW r13, r11, 2 | 00000000 00010 01011 010 01101 0000011 | 0x025A293 | I |
| SLL r15, r11, r2 | 00000000 00010 01011 001 01111 0110011 | 0x025A933 | R |



### Analyzing the instructions


### 1. **ADD r5, r6, r7** (R-type)
   - **Opcode:** `0110011` (R-type)
   - **rd:** `r5` (`00101`)
   - **funct3:** `000` (for ADD)
   - **rs1:** `r6` (`00110`)
   - **rs2:** `r7` (`00111`)
   - **funct7:** `0000000`
   - **Binary Encoding:** `0000000 00111 00110 000 00101 0110011`

### 2. **SUB r7, r5, r6** (R-type)
   - **Opcode:** `0110011` (R-type)
   - **rd:** `r7` (`00111`)
   - **funct3:** `000` (for SUB)
   - **rs1:** `r5` (`00101`)
   - **rs2:** `r6` (`00110`)
   - **funct7:** `0100000`
   - **Binary Encoding:** `0100000 00110 00101 000 00111 0110011`

### 3. **AND r6, r5, r7** (R-type)
   - **Opcode:** `0110011` (R-type)
   - **rd:** `r6` (`00110`)
   - **funct3:** `111` (for AND)
   - **rs1:** `r5` (`00101`)
   - **rs2:** `r7` (`00111`)
   - **funct7:** `0000000`
   - **Binary Encoding:** `0000000 00111 00101 111 00110 0110011`

### 4. **OR r8, r6, r5** (R-type)
   - **Opcode:** `0110011` (R-type)
   - **rd:** `r8` (`01000`)
   - **funct3:** `110` (for OR)
   - **rs1:** `r6` (`00110`)
   - **rs2:** `r5` (`00101`)
   - **funct7:** `0000000`
   - **Binary Encoding:** `0000000 00101 00110 110 01000 0110011`

### 5. **XOR r8, r5, r4** (R-type)
   - **Opcode:** `0110011` (R-type)
   - **rd:** `r8` (`01000`)
   - **funct3:** `100` (for XOR)
   - **rs1:** `r5` (`00101`)
   - **rs2:** `r4` (`00100`)
   - **funct7:** `0000000`
   - **Binary Encoding:** `0000000 00100 00101 100 01000 0110011`

### 6. **SLT r10, r2, r4** (R-type)
   - **Opcode:** `0110011` (R-type)
   - **rd:** `r10` (`01010`)
   - **funct3:** `010` (for SLT)
   - **rs1:** `r2` (`00010`)
   - **rs2:** `r4` (`00100`)
   - **funct7:** `0000000`
   - **Binary Encoding:** `0000000 00100 00010 010 01010 0110011`

### 7. **ADDI r12, r3, 5** (I-type)
   - **Opcode:** `0010011` (I-type)
   - **rd:** `r12` (`01100`)
   - **funct3:** `000` (for ADDI)
   - **rs1:** `r3` (`00011`)
   - **imm[11:0]:** `000000000101` (Immediate value = 5)
   - **Binary Encoding:** `000000000101 00011 000 01100 0010011`

### 8. **SW r3, r1, 4** (S-type)
   - **Opcode:** `0100011` (S-type)
   - **imm[11:5]:** `0000000` (High 7 bits of the immediate)
   - **rs2:** `r3` (`00011`)
   - **rs1:** `r1` (`00001`)
   - **funct3:** `010` (for SW)
   - **imm[4:0]:** `00100` (Low 5 bits of the immediate, 4 in decimal)
   - **Binary Encoding:** `0000000 00011 00001 010 00100 0100011`

### 9. **SRL r16, r11, r2** (R-type)
   - **Opcode:** `0110011` (R-type)
   - **rd:** `r16` (`10000`)
   - **funct3:** `101` (for SRL)
   - **rs1:** `r11` (`01011`)
   - **rs2:** `r2` (`00010`)
   - **funct7:** `0000000`
   - **Binary Encoding:** `0000000 00010 01011 101 10000 0110011`

### 10. **BNE r0, r1, 20** (B-type)
   - **Opcode:** `1100011` (B-type)
   - **imm[12|10:5]:** `0000010` (High bits of the immediate, `00010`)
   - **rs2:** `r1` (`00001`)
   - **rs1:** `r0` (`00000`)
   - **funct3:** `001` (for BNE)
   - **imm[4:1|11]:** `00010` (Low bits of the immediate, `00100` for 20)
   - **Binary Encoding:** `0000010 00001 00000 001 00010 1100011`

### 11. **BEQ r0, r0, 15** (B-type)
   - **Opcode:** `1100011` (B-type)
   - **imm[12|10:5]:** `0000001` (High bits of the immediate, `00001`)
   - **rs2:** `r0` (`00000`)
   - **rs1:** `r0` (`00000`)
   - **funct3:** `000` (for BEQ)
   - **imm[4:1|11]:** `11110` (Low bits of the immediate, `01110` for 15)
   - **Binary Encoding:** `0000001 00000 00000 000 11110 1100011`

### 12. **LW r13, r11, 2** (I-type)
   - **Opcode:** `0000011` (I-type)
   - **rd:** `r13` (`01101`)
   - **funct3:** `010` (for LW)
   - **rs1:** `r11` (`01011`)
   - **imm[11:0]:** `000000000010` (Immediate value = 2)
   - **Binary Encoding:** `000000000010 01011 010 01101 0000011`

### 13. **SLL r15, r11, r2** (R-type)
   - **Opcode:** `0110011` (R-type)
   - **rd:** `r15` (`01111`)
   - **funct3:** `001` (for SLL)
   - **rs1:** `r11` (`01011`)
   - **rs2:** `r2` (`00010`)
   - **funct7:** `0000000`
   - **Binary Encoding:** `0000000 00010 01011 001 01111 0110011`

</details>

<details>
<summary>LAB 5: RISC-V ISA in the referenced verilog code and the output wave form.</summary>

### Functional simulation : Downloaded the files and then use the following commands.

1. ```iverilog  -0 dump iiitb_rv321.v iiitb_rv321_tb.v```
2. ```./dump```
3. ```gtkwave iiitb_rv321.vcd```

<p align="left">
  <img width="750" alt="image" src="https://github.com/user-attachments/assets/cd4f0277-00ae-4bcd-8752-2bd935b748ee">
</p

<p align="left">
  <img width="750" alt="image" src="https://github.com/user-attachments/assets/4d6b8494-d1fa-4a28-ac7a-9527b2e55165">
</p

Table

| S. No. | Operation         | Standard RISCV ISA | Hardcoded ISA   |
|--------|-------------------|--------------------|-----------------|
| 1      | ADD R6, R2, R1    | 32'h00110333       | 32'h02208300    |
| 2      | SUB R7, R1, R2    | 32'h402083b3       | 32'h02209380    |
| 3      | AND R8, R1, R3    | 32'h0030f433       | 32'h0230a400    |
| 4      | OR R9, R2, R5     | 32'h005164b3       | 32'h02513480    |
| 5      | XOR R10, R1, R4   | 32'h0040c533       | 32'h0240c500    |
| 6      | SLT R1, R2, R4    | 32'h0045a0b3       | 32'h02415580    |
| 7      | ADDI R12, R4, 5   | 32'h004120b3       | 32'h00520600    |
| 8      | BEQ R0, R0, 15    | 32'h00000f63       | 32'h00f00002    |
| 9      | SW R3, R1, 2      | 32'h0030a123       | 32'h00209181    |
| 10     | LW R13, R1, 2     | 32'h0020a683       | 32'h00208681    |
| 11     | SRL R16, R14, R2  | 32'h0030a123       | 32'h00271803    |
| 12     | SLL R15, R1, R2   | 32'h002097b3       | 32'h00208783    |




## Waveform : The output waveforms of the instructions in 5-satge pipeline architecture


```1. ADD R6, R2, R1```
<p align="left">
  <img width="843" alt="image" src="https://github.com/user-attachments/assets/1e7945e3-63f2-47e6-970a-a14a25970e2e">

</p

```2. SUB R7, R1, R2```
<p align="left">
 <img width="843" alt="image" src="https://github.com/user-attachments/assets/91d4e37b-9fca-477e-a2cc-4bece753dc47">
</p

```3.  AND R8, R1, R3```
<p align="left">
 <img width="843" alt="image" src="https://github.com/user-attachments/assets/87d256f1-ded3-416e-9e66-54703a0056a9">
</p

```4.  OR R9, R2, R5```
<p align="left">
 <img width="839" alt="image" src="https://github.com/user-attachments/assets/39a8905c-1172-4e78-bdc9-b3e494aa57e1">
</p

```5.  XOR R10, R1, R4```
<p align="left">
 <img width="839" alt="image" src="https://github.com/user-attachments/assets/05773ea3-530b-4d0c-bb21-687d11efb3fb">
</p

```6.  SLT R1, R2, R4```
<p align="left">
 <img width="842" alt="image" src="https://github.com/user-attachments/assets/669f782f-01ef-410e-9f2b-e43aa8a36c47">
</p

```7.  ADDI R12, R4, 5```
<p align="left">
 <img width="842" alt="image" src="https://github.com/user-attachments/assets/f4745a6d-d3ac-42e7-9101-579d1611dfa7">
</p

```8.  BEQ R0, R0, 15```
<p align="left">
 <img width="839" alt="image" src="https://github.com/user-attachments/assets/c7cbfcd9-aa85-4b3f-9c8d-eb1d86077220">
</p

```9.  SW R3, R1, 2```
<p align="left">
 <img width="841" alt="image" src="https://github.com/user-attachments/assets/ec9cc5a7-de58-426d-8bd2-1200ab73e565">
</p

```10.  LW R13, R1, 2```
<p align="left">
  <img width="842" alt="image" src="https://github.com/user-attachments/assets/cf23cb17-89f3-403e-a047-296951dfd068">
</p>


</details>

<details>
<summary> LAB 6 : EuclidMax : GCD using Euclidean Algorithm</summary>

# EuclidMax : GCD using Euclidean Algorithm

The GCD using the Euclidean algorithm is a highly efficient method for computing the greatest common divisor of two integers. Based on the principle that the GCD of two numbers also divides their difference, this algorithm repeatedly replaces the larger number with the remainder of dividing the two numbers until one of them becomes zero. The other number, at this point, is the GCD. This approach significantly reduces the size of the numbers involved, making it particularly effective for large integers. The Euclidean algorithm's time complexity of \(O(\log(\min(a, b)))\) ensures rapid computation, and its straightforward iterative process makes it a popular choice for calculating the GCD in various applications.

`EuclidMax.c` is the file containing code to calculate the GCD using Euclidean Algorithm.

<p align="left">
 <img width="842" alt="image" src="https://github.com/user-attachments/assets/4b5ce89e-3308-42ab-b0a6-752e3d6d476b">
</p>

### Compiling the code using GCC compiler :

compiling the `EuclidMax.c` with `gcc EuclidMax.c` and run the executable file `./a.out`

<p align="left">
 <img width="839" alt="image" src="https://github.com/user-attachments/assets/ef180b23-f69d-4d3f-8133-574fd6dfb88f">
</p>

Output for GCD of numbers 48 and 18 is 6, shown in the above image.

### Compiling the code using RISC-V compiler :

compiling the `EuclidMax.c` using the commands :

```bash
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o EuclidMax.o EuclidMax.c
```

```bash
spike pk EuclidMax.o
```

<p align="left">
  <img width="842" alt="image" src="https://github.com/user-attachments/assets/8a806c4a-fa75-4ea7-84d6-f87d76d8e7af">
</p>

Output for the GCD of 48 and 18 using Euclidean Algorithm is same in both GCC compiler and RISC-V compiler.

</details>

<details>
<summary> LAB 7 : Digital logic with TL-verilog and Makerchip, RISCV CPU microarchitecture and pipelined RISCV CPU microarchitecture </summary>

## Day 3

TL-Verilog simplifies digital circuit design by eliminating legacy Verilog features and introducing a streamlined syntax with powerful constructs for pipelines and transactions. Makerchip, a cloud-based platform, accepts TL-Verilog code and automatically generates logical diagrams and waveforms without needing an external testbench. This section discusses TL-Verilog, the Makerchip platform, and provides relevant examples.

<p align="left">
  <img width="750" alt="image" src="https://github.com/user-attachments/assets/cd0edb1c-487f-4e65-be74-f0daedced905">
</p>

<p align="left">
  <img width="750" alt="image" src="https://github.com/user-attachments/assets/a413128f-62a4-404a-bf58-2308486431fa">
</p>

1. Inverter

<p align="left">
  <img width="959" alt="image" src="https://github.com/user-attachments/assets/8de60122-a6bc-40fa-a388-7480b0ad61ff">
</p>

2.  2-input AND gate

<p align="left">
  <img width="959" alt="image" src="https://github.com/user-attachments/assets/c248e879-c607-4b2f-ba55-dbfc041d39ff">
</p>

3. 2-input OR gate

<p align="left">
  <img width="959" alt="image" src="https://github.com/user-attachments/assets/eff64204-5128-4437-ba9e-68c9df228113">
</p>

4. 2:1 MUX

<p align="left">
  <img width="957" alt="image" src="https://github.com/user-attachments/assets/e7a33453-64d9-4351-a9a3-d180e91f43ff">
</p>

5. 2:1 MUX using vectors

<p align="left">
  <img width="958" alt="image" src="https://github.com/user-attachments/assets/b9267fe5-aee4-4998-9836-81e031babb70">
</p>

6. Basic calculator

<p align="left">
 <img width="958" alt="image" src="https://github.com/user-attachments/assets/666b432a-c7ec-4f69-98b3-6292738601ff">
</p>

7. Counter

<p align="left">
 <img width="959" alt="image" src="https://github.com/user-attachments/assets/6fbe6724-3bd9-4477-a207-c237fad038cb">
</p>
 
8. Sequencial counter

<p align="left">
  <img width="959" alt="image" src="https://github.com/user-attachments/assets/27158a8f-18ad-4f01-aa3c-a9f5d955573c">
</p>

9. Fibonacci series

<p align="left">
  <img width="955" alt="image" src="https://github.com/user-attachments/assets/f9fa4a16-b385-42c1-b3e1-12d807d81c12">
</p>

### Pipelined logic

In TL-Verilog, pipelined logic is efficiently modeled using pipeline constructs that represent data flow across design stages, each linked to a clock cycle. This approach simplifies sequential logic by automating state propagation and enabling clear, concise descriptions of multi-stage operations. By focusing on transaction flow, TL-Verilog enhances design clarity, maintainability, and verification, making it easier to manage complex digital designs.


<p align="left">
  <img width="813" alt="image" src="https://github.com/user-attachments/assets/5f15225a-1eb8-48d3-b3c8-e94e608f0538">
</p>

1. Pipenlined design


<p align="left">
  <img width="954" alt="image" src="https://github.com/user-attachments/assets/a97f64b6-6fe8-4ec0-befc-12a47575fa24">
</p>

2. Pipelined calculator

<p align="left">
  <img width="957" alt="image" src="https://github.com/user-attachments/assets/f17831de-e254-4276-ab62-8f8f03851c72">
</p>

3. Cycle calculator

<p align="left">
  <img width="958" alt="image" src="https://github.com/user-attachments/assets/767acd23-de04-4c85-b1c1-fba7472f4ad3">
</p>

### Day 4

#### RISC-V micro architecture

The functional specification of a processor is referred to  the term architecture. It represents the capabilities that the programme can rely on from the hardware. Architecture does not describe how a processor is constructed. It describes what a processor is capable of. Micro-architecture, on the other hand, describes the construction and design of a processor. The number and size of caches, instruction cycle counts, pipeline length, and other parameters are defined by microarchitecture.

<p align="left">
  <img width="449" alt="image" src="https://github.com/user-attachments/assets/fee92f9b-9dd9-4209-b4dd-113c2aca64f2">
</p>

<p align="left">
  <img width="462" alt="image" src="https://github.com/user-attachments/assets/5fcf7385-6a7b-4484-a714-8ecfab0f5314">
</p>

<p align="left">
 <img width="959" alt="image" src="https://github.com/user-attachments/assets/f9d36ad1-70a0-47a9-ab80-a9be765dde40">
</p>

1. PC LAB

The Program Counter holds the address of the next instruction to execute, incrementing by 4 each time. On reset, it reinitializes to zero before continuing instruction execution.
<p align="left">
 <img width="956" alt="image" src="https://github.com/user-attachments/assets/d2da0ff7-5742-432a-8ffa-6128775928bd">
</p>

2. Instruction fetch

In a computer, the program counter (PC) holds the address of the next instruction to be fetched from the instruction memory (IM). The PC is incremented by 4 after each valid iteration. The instruction fetched from the IM is 32-bits wide and is used by the processor for execution during the fetch stage.

<p align="left">
  <img width="959" alt="image" src="https://github.com/user-attachments/assets/f3ff5f35-6887-40cf-a8a6-f7bcfea37622">
</p>

3. Decode


The fetched 32-bit instruction is decoded to determine the operation, source, and destination addresses. Instructions are classified into 6 types: R, I, S, B, U, and J. The decoded instruction specifies the opcode, immediate value, and register addresses. The RISC-V ISA provides 32 registers, allowing 2 reads and 1 write simultaneously during the decode stage.

<p align="left">
<img width="500" alt="image" src="https://github.com/user-attachments/assets/ba6f4bb6-fde8-4536-b630-828477401704">
</p>

<p align="left">
  <img width="959" alt="image" src="https://github.com/user-attachments/assets/89f69b4b-f028-408c-bc76-3b5a4c1414c6">
</p>

3.a Immediate Decode Logic

<p align="left">
 <img width="600" alt="image" src="https://github.com/user-attachments/assets/13c9bfc9-b494-4bb9-bc24-b29fdfcbbf04">
</p>


<p align="left">
 <img width="959" alt="image" src="https://github.com/user-attachments/assets/eb76d59c-5f94-4b95-a84c-d9f7b1db10a8">
</p>


3.b Decode logic for other fields like rs1,rs2,func3,func7

<p align="left">
  <img width="600" alt="image" src="https://github.com/user-attachments/assets/ede8dac8-8183-4388-970e-b41196593f82">
</p>

<img width="959" alt="image" src="https://github.com/user-attachments/assets/74493771-5a0a-44e7-9cd0-acb7e1c4a176">



<p align="left">
  <img width="959" alt="image" src="https://github.com/user-attachments/assets/3c3f3d6b-1708-4a5f-b124-50ca2e2f9080">
</p>

3.c Decoding Individual Instruction

<img width="327" alt="image" src="https://github.com/user-attachments/assets/9f0b2b41-c46a-4160-951c-ec0c7e6daf76">


<p align="left">
  <img width="956" alt="image" src="https://github.com/user-attachments/assets/4d3a32d3-acbe-476d-aaf0-6bcce545d1fa">
</p>

4. Register file read and enable

<img width="470" alt="image" src="https://github.com/user-attachments/assets/02cc379c-1192-4659-bf26-e122e849a866">


<p align="left">
 <img width="959" alt="image" src="https://github.com/user-attachments/assets/21b63825-f521-4c5f-916a-814361b182d3">
</p>

5. Arithmetic and logical unit

<img width="482" alt="image" src="https://github.com/user-attachments/assets/12dbdfee-9b12-4376-9a90-fc267c0d56fa">


<p align="left">
 <img width="959" alt="image" src="https://github.com/user-attachments/assets/bcc0ef0f-cf12-4dfb-b7d0-605750fac35e">
</p>

6. Register File Write

<img width="478" alt="image" src="https://github.com/user-attachments/assets/bdc14112-054a-433f-8b1f-094a47807998">

<img width="959" alt="image" src="https://github.com/user-attachments/assets/9ffbb81b-7708-4278-91b1-36d787d8f4b0">

7. Branch Instruction

<img width="548" alt="image" src="https://github.com/user-attachments/assets/fe24f274-8f9c-4f98-a4fd-ef293b219882">


<p align="left">
 <img width="959" alt="image" src="https://github.com/user-attachments/assets/e4909163-810e-40c6-b9fa-b48637eceb95">
</p>


#### complete code

```bash
\m4_TLV_version 1d: tl-x.org
\SV
   // This code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   
   m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop/master/tlv_lib/risc-v_shell_lib.tlv'])

\SV
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV

   m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
   // Function:
   m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
   m4_asm(ADDI, r12, r10, 1011)         // Store count of 10 in register a2.
   m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   // Loop:
   m4_asm(ADD, r14, r13, r14)           // Incremental addition
   m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
   m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
   m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
   
   //m4_asm(SW, r0, r10 ,100)
   //m4_asm(LW, r15, r0, 100)
   // Optional:
   // m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;
         
         $clk_yas = *clk;
         //$pc[31:0] = >>1$reset ? 0 : ( >>1$pc + 32'h4 );
         
         $pc[31:0] = >>1$reset ? 32'b0 :
                     >>1$taken_branch ? >>1$br_target_pc :
                     >>1$pc + 32'd4;
         
         $imem_rd_en = >>1$reset ? 0 : 1;
         $imem_rd_addr[31:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
      @1
         $instr[31:0] = $imem_rd_data[31:0];
         
         $is_i_instr = $instr[6:2] ==? 5'b0000x || $instr[6:2] ==? 5'b001x0 || $instr[6:2] ==? 5'b11001;
         $is_r_instr = $instr[6:2] ==? 5'b01011 || $instr[6:2] ==? 5'b011x0 || $instr[6:2] ==? 5'b10100;
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                      $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                      $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                      $is_u_instr ? {$instr[31:12], 12'b0} :
                      $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                      32'b0;
         
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
            
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
            
         $funct7_valid = $is_r_instr ;
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
            
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
            
         $opcode[6:0] = $instr[6:0];
         
         $dec_bits [10:0] = {$funct7[5], $funct3, $opcode};
         
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
         
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         $src1_value[31:0] = $rf_rd_data1;
         $src2_value[31:0] = $rf_rd_data2;
         
         
         $result[31:0] = $is_addi ? $src1_value + $imm :
                         $is_add ? $src1_value + $src2_value :
                         32'bx ;
         
         $rf_wr_en = $rd_valid && $rd != 5'b0;
         $rf_wr_index[4:0] = $rd;
         $rf_wr_data[31:0] = $result;
         
         
         $taken_branch = $is_beq ? ($src1_value == $src2_value):
                         $is_bne ? ($src1_value != $src2_value):
                         $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bltu ? ($src1_value < $src2_value):
                         $is_bgeu ? ($src1_value >= $src2_value):
                         1'b0;
         $br_target_pc[31:0] = $pc +$imm;
      
   
   
   // Assert these to end simulation (before Makerchip cycle limit).
   
   *passed = *cyc_cnt > 40;
   *passed = |cpu/xreg[14]>>5$value == (1+2+3+4+5+6+7+8+9+10) ;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
      //m4+dmem(@4)    // Args: (read/write stage)
   
   m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic. @4 would work for all labs.
\SV
   endmodule
```

#### Testbench

```bash
*passed = |cpu/xreg[10]>>5$value == (1+2+3+4+5+6+7+8+9) ;
```

Checking the result in the log file

<img width="556" alt="image" src="https://github.com/user-attachments/assets/a4e1113f-0287-4b6f-919e-e8f9b05cda39">

#### Observation

clk

<img width="527" alt="image" src="https://github.com/user-attachments/assets/94c0bbea-e074-4d8c-aae0-714fb6f85d70">

Reset

<img width="533" alt="image" src="https://github.com/user-attachments/assets/295180ae-3c70-4345-bdaf-196ecf49f2bc">

Waveforms showing gradual addition from 1 to 9, that is from 0(h00) to 45(h2d):

<p align="left">
  <img width="950" alt="image" src="https://github.com/user-attachments/assets/11c46fc2-643b-4d7a-9a67-7641f1c8ad1f">
</p>




## Day 5

#### CPU Pipeline 

Pipelining the CPU core enables easier retiming and reduces functional bugs. It allows for faster computation by breaking the execution process into stages. In the pipelined architecture, each stage is denoted with @1, @2, and so on. The benefit of using TL-Verilog is that defining the pipeline stages in a systematic order is not necessary, providing more flexibility in the design.

Structural hazards occur when multiple instructions compete for the same hardware resources, leading to pipeline stalls until the conflict is resolved. Data hazards arise from dependencies on results from previous instructions, potentially causing incorrect outcomes if the required data is not yet available. Control hazards, or branch hazards, stem from uncertainty over branch outcomes, which can delay confirmation until the execution stage and result in incorrect instruction fetches and performance penalties from pipeline flushing. Identifying and mitigating these hazards is crucial for ensuring the efficient and correct operation of a pipelined CPU.

#### valid signal for pipelined logic

```bash
$start = >>1$reset && !$reset;
$valid = $reset ? 1'b0 : ($start || >>3$valid);
$valid_or_reset = $valid || $reset;
$rs1_or_funct3_valid    = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
$rs2_valid              = $is_r_instr || $is_s_instr || $is_b_instr;
$rd_valid               = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
$funct7_valid           = $is_r_instr;
```

Valid Signal for Pipelined Logic:

The $valid signal is set to 0 on reset and becomes 1 when the $start signal is true or when the previous stage's $valid signal is true.
The $valid_or_reset signal is the logical OR of the $valid and $reset signals.
The $rs1_or_funct3_valid, $rs2_valid, $rd_valid, and $funct7_valid signals are set based on the type of instruction (R, I, S, B, U, J).

#### Handling Datahazards in Register file with bypassing

```bash
$src1_value[31:0] = $rs1_bypass ? >>1$result[31:0] : $rf_rd_data1[31:0];
$src2_value[31:0] = $rs2_bypass ? >>1$result[31:0] : $rf_rd_data2[31:0];
```

The $valid_taken_br, $valid_load, $jump_valid, $jal_valid, and $jalr_valid signals are set based on the current instruction's validity and type.

#### Correcting branch target path:

```bash
   //Current instruction is valid if one of the previous 2 instructions were not (taken_branch or load or jump)
   $valid = ~(>>1$valid_taken_br || >>2$valid_taken_br || >>1$is_load || >>2$is_load || >>2$jump_valid 	|| >>1$jump_valid);
         
   //Current instruction is valid & is a taken branch
   $valid_taken_br = $valid && $taken_br;
         
   //Current instruction is valid & is a load
   $valid_load = $valid && $is_load;
         
   //Current instruction is valid & is jump
   $jump_valid = $valid && $is_jump;
   $jal_valid  = $valid && $is_jal;
   $jalr_valid = $valid && $is_jalr;
    
    *passed = |cpu/xreg[14]>>5$value == (1+2+3+4+5+6+7+8+9+10);

The *passed signal checks if the sum of the values in register 14 after 5 stages is correct.

```

#### RISC-V CPU

<img width="604" alt="image" src="https://github.com/user-attachments/assets/2c90f24e-8a27-429b-9e70-20b7ad7e0b6e">

```bash
\m4_TLV_version 1d: tl-x.org
\SV
   // Template code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   
   m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop/master/tlv_lib/risc-v_shell_lib.tlv'])

\SV
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV

   // /====================\
   // | Sum 0 to 9 Program |
   // \====================/
   //
   // Add 0,1,2,3,...,9 (in that order).
   //
   // Regs:
   //  r10 (a0): In: 0, Out: final sum
   //  r12 (a2): 10
   //  r13 (a3): 1..10
   //  r14 (a4): Sum
   // 
   // External to function:
   m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
   // Function:
   m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
   m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
   m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   // Loop:
   m4_asm(ADD, r14, r13, r14)           // Incremental addition
   m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
   m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
   m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
   m4_asm(SW, r0, r10, 10000)           // Store r10 result in dmem
   m4_asm(LW, r17, r0, 10000)           // Load contents of dmem to r17
   m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;
         $clk_CHA = *clk;
         
         //PC fetch - branch, jumps and loads introduce 2 cycle bubbles in this pipeline
         $pc[31:0] = >>1$reset ? '0 : (>>3$valid_taken_br ? >>3$br_tgt_pc :
                                       >>3$valid_load     ? >>3$inc_pc[31:0] :
                                       >>3$jal_valid      ? >>3$br_tgt_pc :
                                       >>3$jalr_valid     ? >>3$jalr_tgt_pc :
                                                     (>>1$inc_pc[31:0]));
         // Access instruction memory using PC
         $imem_rd_en = ~ $reset;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         
         
      @1
         //Getting instruction from IMem
         $instr[31:0] = $imem_rd_data[31:0];
         
         //Increment PC
         $inc_pc[31:0] = $pc[31:0] + 32'h4;
         
         //Decoding I,R,S,U,B,J type of instructions based on opcode [6:0]
         //Only [6:2] is used here because this implementation is for RV64I which does not use [1:0]
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] == 5'b11001;
         
         $is_r_instr = $instr[6:2] == 5'b01011 ||
                       $instr[6:2] ==? 5'b011x0 ||
                       $instr[6:2] == 5'b10100;
         
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         $is_b_instr = $instr[6:2] == 5'b11000;
         
         $is_j_instr = $instr[6:2] == 5'b11011;
         
         //Immediate value decode
         $imm[31:0] = $is_i_instr ? { {21{$instr[31]}} , $instr[30:20]} :
                      $is_s_instr ? { {21{$instr[31]}} , $instr[30:25] , $instr[11:8] , $instr[7]} :
                      $is_b_instr ? { {20{$instr[31]}} , $instr[7] , $instr[30:25] , $instr[11:8] , 1'b0} :
                      $is_u_instr ? { $instr[31] , $instr[30:12] , { 12{1'b0}} } :
                      $is_j_instr ? { {12{$instr[31]}} , $instr[19:12] , $instr[20] , $instr[30:21] , 1'b0} :
                      >>1$imm[31:0];
         
         //Generate valid signals for each instruction fields
         $rs1_or_funct3_valid    = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $rs2_valid              = $is_r_instr || $is_s_instr || $is_b_instr;
         $rd_valid               = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct7_valid           = $is_r_instr;
         
         //Decode other fields of instruction - source and destination registers, funct, opcode
         ?$rs1_or_funct3_valid
            $rs1[4:0]    = $instr[19:15];
            $funct3[2:0] = $instr[14:12];
         
         ?$rs2_valid
            $rs2[4:0]    = $instr[24:20];
         
         ?$rd_valid
            $rd[4:0]     = $instr[11:7];
         
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
         
         $opcode[6:0] = $instr[6:0];
         
         //Decode instruction in subset of base instruction set based on RISC-V 32I
         $dec_bits[10:0] = {$funct7[5],$funct3,$opcode};
         
         //Branch instructions
         $is_beq   = $dec_bits ==? 11'bx_000_1100011;
         $is_bne   = $dec_bits ==? 11'bx_001_1100011;
         $is_blt   = $dec_bits ==? 11'bx_100_1100011;
         $is_bge   = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu  = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu  = $dec_bits ==? 11'bx_111_1100011;
         
         //Jump instructions
         $is_auipc = $dec_bits ==? 11'bx_xxx_0010111;
         $is_jal   = $dec_bits ==? 11'bx_xxx_1101111;
         $is_jalr  = $dec_bits ==? 11'bx_000_1100111;
         
         //Arithmetic instructions
         $is_addi  = $dec_bits ==? 11'bx_000_0010011;
         $is_add   = $dec_bits ==  11'b0_000_0110011;
         $is_lui   = $dec_bits ==? 11'bx_xxx_0110111;
         $is_slti  = $dec_bits ==? 11'bx_010_0010011;
         $is_sltiu = $dec_bits ==? 11'bx_011_0010011;
         $is_xori  = $dec_bits ==? 11'bx_100_0010011;
         $is_ori   = $dec_bits ==? 11'bx_110_0010011;
         $is_andi  = $dec_bits ==? 11'bx_111_0010011;
         $is_slli  = $dec_bits ==? 11'b0_001_0010011;
         $is_srli  = $dec_bits ==? 11'b0_101_0010011;
         $is_srai  = $dec_bits ==? 11'b1_101_0010011;
         $is_sub   = $dec_bits ==? 11'b1_000_0110011;
         $is_sll   = $dec_bits ==? 11'b0_001_0110011;
         $is_slt   = $dec_bits ==? 11'b0_010_0110011;
         $is_sltu  = $dec_bits ==? 11'b0_011_0110011;
         $is_xor   = $dec_bits ==? 11'b0_100_0110011;
         $is_srl   = $dec_bits ==? 11'b0_101_0110011;
         $is_sra   = $dec_bits ==? 11'b1_101_0110011;
         $is_or    = $dec_bits ==? 11'b0_110_0110011;
         $is_and   = $dec_bits ==? 11'b0_111_0110011;
         
         //Store instructions
         $is_sb    = $dec_bits ==? 11'bx_000_0100011;
         $is_sh    = $dec_bits ==? 11'bx_001_0100011;
         $is_sw    = $dec_bits ==? 11'bx_010_0100011;
         
         //Load instructions - support only 4 byte load
         $is_load  = $dec_bits ==? 11'bx_xxx_0000011;
         
         $is_jump = $is_jal || $is_jalr;
         
      @2
         //Get Source register values from reg file
         $rf_rd_en1 = $rs1_or_funct3_valid;
         $rf_rd_en2 = $rs2_valid;
         
         $rf_rd_index1[4:0] = $rs1[4:0];
         $rf_rd_index2[4:0] = $rs2[4:0];
         
         //Register file bypass logic - data forwarding from ALU to resolve RAW dependence
         $src1_value[31:0] = $rs1_bypass ? >>1$result[31:0] : $rf_rd_data1[31:0];
         $src2_value[31:0] = $rs2_bypass ? >>1$result[31:0] : $rf_rd_data2[31:0];
         
         //Branch target PC computation for branches and JAL
         $br_tgt_pc[31:0] = $imm[31:0] + $pc[31:0];
         
         //RAW dependence check for ALU data forwarding
         //If previous instruction was writing to reg file, and current instruction is reading from same register
         $rs1_bypass = >>1$rf_wr_en && (>>1$rd == $rs1);
         $rs2_bypass = >>1$rf_wr_en && (>>1$rd == $rs2);
         
      @3
         //ALU
         $result[31:0] = $is_addi  ? $src1_value +  $imm :
                         $is_add   ? $src1_value +  $src2_value :
                         $is_andi  ? $src1_value &  $imm :
                         $is_ori   ? $src1_value |  $imm :
                         $is_xori  ? $src1_value ^  $imm :
                         $is_slli  ? $src1_value << $imm[5:0]:
                         $is_srli  ? $src1_value >> $imm[5:0]:
                         $is_and   ? $src1_value &  $src2_value:
                         $is_or    ? $src1_value |  $src2_value:
                         $is_xor   ? $src1_value ^  $src2_value:
                         $is_sub   ? $src1_value -  $src2_value:
                         $is_sll   ? $src1_value << $src2_value:
                         $is_srl   ? $src1_value >> $src2_value:
                         $is_sltu  ? $sltu_rslt[31:0]:
                         $is_sltiu ? $sltiu_rslt[31:0]:
                         $is_lui   ? {$imm[31:12], 12'b0}:
                         $is_auipc ? $pc + $imm:
                         $is_jal   ? $pc + 4:
                         $is_jalr  ? $pc + 4:
                         $is_srai  ? ({ {32{$src1_value[31]}} , $src1_value} >> $imm[4:0]) :
                         $is_slt   ? (($src1_value[31] == $src2_value[31]) ? $sltu_rslt : {31'b0, $src1_value[31]}):
                         $is_slti  ? (($src1_value[31] == $imm[31]) ? $sltiu_rslt : {31'b0, $src1_value[31]}) :
                         $is_sra   ? ({ {32{$src1_value[31]}}, $src1_value} >> $src2_value[4:0]) :
                         $is_load  ? $src1_value +  $imm :
                         $is_s_instr ? $src1_value + $imm :
                                    32'bx;
         
         $sltu_rslt[31:0]  = $src1_value <  $src2_value;
         $sltiu_rslt[31:0] = $src1_value <  $imm;
         
         //Jump instruction target PC computation
         $jalr_tgt_pc[31:0] = $imm[31:0] + $src1_value[31:0]; 
         
         //Branch resolution
         $taken_br = $is_beq ? ($src1_value == $src2_value) :
                     $is_bne ? ($src1_value != $src2_value) :
                     $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                     $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                     $is_bltu ? ($src1_value < $src2_value) :
                     $is_bgeu ? ($src1_value >= $src2_value) :
                     1'b0;
         
         //Current instruction is valid if one of the previous 2 instructions were not (taken_branch or load or jump)
         $valid = ~(>>1$valid_taken_br || >>2$valid_taken_br || >>1$is_load || >>2$is_load || >>2$jump_valid || >>1$jump_valid);
         
         //Current instruction is valid & is a taken branch
         $valid_taken_br = $valid && $taken_br;
         
         //Current instruction is valid & is a load
         $valid_load = $valid && $is_load;
         
         //Current instruction is valid & is jump
         $jump_valid = $valid && $is_jump;
         $jal_valid  = $valid && $is_jal;
         $jalr_valid = $valid && $is_jalr;
         
         //Destination register update - ALU result or load result depending on instruction
         $rf_wr_en = (($rd != '0) && $rd_valid && $valid) || >>2$valid_load;
         $rf_wr_index[4:0] = $valid ? $rd[4:0] : >>2$rd[4:0];
         $rf_wr_data[31:0] = $valid ? $result[31:0] : >>2$ld_data[31:0];
         
      @4
         //Data memory access for load, store
         $dmem_addr[3:0]     =  $result[5:2];
         $dmem_wr_en         =  $valid && $is_s_instr;
         $dmem_wr_data[31:0] =  $src2_value[31:0];
         $dmem_rd_en         =  $valid_load;
         
      
         //Write back data read from load instruction to register
         $ld_data[31:0]      =  $dmem_rd_data[31:0];
         
      
      

      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

   
   // Assert these to end simulation (before Makerchip cycle limit).
   //Checks if sum of numbers from 1 to 9 is obtained in reg[17] and runs 10 cycles extra after this is met
   *passed = |cpu/xreg[17]>>10$value == (1+2+3+4+5+6+7+8+9);
   //Run for 200 cycles without any checks
   //*passed = *cyc_cnt > 200;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@2, @3)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+dmem(@4)    // Args: (read/write stage)
   
   m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic
                       // @4 would work for all labs
\SV
   endmodule
```

<img width="897" alt="image" src="https://github.com/user-attachments/assets/aca2fdec-db4f-4bd9-87f0-344edc0b53f1">


LOG

<img width="527" alt="image" src="https://github.com/user-attachments/assets/27ae1307-eca7-47e1-936a-ae2520ad7667">

CLC

<img width="532" alt="image" src="https://github.com/user-attachments/assets/cffef344-9f78-4360-9374-8615b35c5fe3">

Reset

<img width="530" alt="image" src="https://github.com/user-attachments/assets/e7b8c947-e7c1-4b43-9c79-32bf04d35901">

Increment of output from 0(h00) to 45(h2d):

<p align="left">
  <img width="517" alt="image" src="https://github.com/user-attachments/assets/ad8d1832-aa1f-4e8c-8dd1-1a594a736e38">
</p>

VIZ

<p align="left">
  <img width="399" alt="image" src="https://github.com/user-attachments/assets/2fdd3dc6-3a40-419b-baa9-2e2e42065899">
</p>


</details>

<details>
<summary>LAB 8: TL Verilog to Verilog, simulation using a testbench and comparing GTK and Makerchip Output </summary>

# TL Verilog to Verilog, simulation using a testbench and comparing GTK and Makerchip Output

The RISC-V processor was initially designed using TL-Verilog in the Makerchip IDE, a platform tailored for digital circuit design and simulation. To implement it on an FPGA, the design was converted to standard Verilog using the Sandpiper-SaaS compiler, which is known for its efficient translation capabilities. Pre-synthesis simulations were subsequently conducted using the GTKWave simulator to thoroughly verify the design's functionality before final synthesis and implementation on the FPGA.

Step by step commands :

```bash 
$ sudo apt install make python python3 python3-pip git iverilog gtkwave
```
```bash
$ cd ~
```
```bash
$ sudo apt-get install python3-venv
```
```bash
$ python3 -m venv .venv
```
```bash
$ source ~/.venv/bin/activate
```
```bash
$ pip3 install pyyaml click sandpiper-saas
```

<p align="left">
  <img width="900" alt="image" src="https://github.com/user-attachments/assets/3b9ba5c0-a696-46f8-8821-01072bbacb1e">
</p>

Installing the packages required to run these set of commands in virtual environment:

```bash
$ sudo apt install make python python3 python3-pip git iverilog gtkwave docker.io
```
```bash
$ sudo chmod 666 /var/run/docker.sock
```
```bash
$ cd ~
```
```bash
$ pip3 install pyyaml click sandpiper-saas
```

<p align="left">
  <img width="841" alt="image" src="https://github.com/user-attachments/assets/638ac0d5-f2d2-45ee-ad40-f30d910f3f1a">
</p>

clone the repository in the required directory and make ```pre_synth_sim``` that will store the output

```bash
$ cd ~
```
```bash
$ git clone https://github.com/manili/VSDBabySoC.git
```
```bash
$ cd /home/vsduser/VSDBabySoC
```
```bash
$ make pre_synth_sim
```

<p align="left">
  <img width="841" alt="image" src="https://github.com/user-attachments/assets/638ac0d5-f2d2-45ee-ad40-f30d910f3f1a">
</p>

Replace the rvmyth.tlv file in the VSDBabySoC/src/module folder with RISC-V design from the Makerchip .tlv file, and update the testbench to align with the Makerchip code.

To generate the Verilog code from TL-Verilog design and convert the .tlv definition of RISC-V into a .v definition, use the following code.


```bash
$ sandpiper-saas -i ./src/module/rvmyth.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module/
```
compile and simulate RISC-V design to run the code

```bash
$ iverilog -o output/pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module
```

<p align="left">
  <img width="900" alt="image" src="https://github.com/user-attachments/assets/468ea3c6-a142-4310-a4a8-25f03f9b5245">
</p>

The Result of the simulation will be stored in the output/pre_synth_sim.

```bash
$ cd output
```
```bash
$ ./pre_synth_sim.out
```

Simualtion in GTK wave

```bash
$ gtkwave pre_synth_sim.vcd
```

<p align="left">
  <img width="900" alt="image" src="https://github.com/user-attachments/assets/c61abf0c-284d-4e25-afc8-bde5c7512e69">
</p>


1.clk_yas: Clock input to the RISC-V core.

2.reset: Input reset signal to the RISC-V core.

3.OUT[9:0]: 10-bit output [9:0] OUT port of the RISC-V core. This port comes from the RISC-V register #14, originally.

GTK wave Plots :

1.clk_yas plot

<p align="left">
  <img width="1300" alt="image" src="https://github.com/user-attachments/assets/961274cb-20f0-4dc4-8f15-b1d611bd9169">
</p>

2. Reset  

<p align="left">
  <img width="1300" alt="image" src="https://github.com/user-attachments/assets/097a91ec-fed9-445b-9d2e-b82a1f04cc30">
</p>

3. OUT[9:0] plot:

<p align="left">
  <img width="1300" alt="image" src="https://github.com/user-attachments/assets/7e5662a5-fc65-4e92-a124-8307f91cad4c">
</p>


Makerchip Plots :

1.clk_yas plot

<p align="left">
  <img width="900" alt="image" src="https://github.com/user-attachments/assets/94c0bbea-e074-4d8c-aae0-714fb6f85d70">
</p>

2. Reset  

<p align="left">
  <img width="900" alt="image" src="https://github.com/user-attachments/assets/295180ae-3c70-4345-bdaf-196ecf49f2bc">
</p>

3. OUT[9:0] plot:

<p align="left">
  <img width="950" alt="image" src="https://github.com/user-attachments/assets/11c46fc2-643b-4d7a-9a67-7641f1c8ad1f">
</p>

</details>

<details>
<summary>LAB 9: Generate waveform for DAC and PLL peripheral for Risc-V processor.</summary>

# Generate waveform for DAC and PLL peripheral for Risc-V processor.

![Screenshot from 2024-09-02 17-44-54](https://github.com/user-attachments/assets/2cfb616a-7e3d-40d6-9540-151ad7c11eb9)

The VSDBabySoC is a compact and robust System-on-Chip (SoC) built on the RISC-V architecture. Its primary objective is to integrate and test three open-source IP cores for the first time while fine-tuning the analog components. This SoC features the RVMYTH microprocessor, an 8x-PLL for generating a stable clock signal, and a 10-bit DAC designed to facilitate communication with other analog devices.

#### BabySoC Simulation

Developing and simulating the complete micro-architecture of a RISC-V CPU is a complex task. For this simulation, we'll focus on incorporating two key IP blocks: PLL and DAC.

#### Phase-Locked Loop (PLL)

A Phase-Locked Loop (PLL) is an electronic system designed to synchronize the phase and frequency of an output signal with a reference signal. It typically consists of three core components: a Phase Detector, which compares the phase of the reference signal with the output signal to generate an error signal; a Loop Filter, which smooths this error signal to reduce noise and enhance system stability; and a Voltage-Controlled Oscillator (VCO), which adjusts its output frequency based on the filtered error signal to minimize the phase difference. PLLs are widely utilized in various applications, including clock generation, frequency synthesis, and data recovery in communication systems.

#### Digital-to-Analog Converter (DAC)

A Digital-to-Analog Converter (DAC) transforms digital signals, typically in binary form, into analog signals like voltage or current. This conversion is vital for systems where digital data must be interpreted by analog devices or presented in a way that can be perceived by humans, such as in audio and video applications. DACs are extensively used in areas like audio playback, video display, and signal processing.

#### step by step procedure

1. Clone this repo: https://github.com/Subhasis-Sahu/BabySoC_Simulation.git using the following command.

``` bash
git clone https://github.com/Subhasis-Sahu/BabySoC_Simulation.git
```
```bash
cd BabySoC_Simulation
```
![Screenshot from 2024-09-02 17-56-30](https://github.com/user-attachments/assets/40f3660b-c598-41a0-a025-3e4da9b64041)

The folder is created after cloning the repo

2. Replacing the rvymth.v file with the required rvymth.v.

![Screenshot from 2024-09-02 17-58-15](https://github.com/user-attachments/assets/4aec49b0-8400-4cb3-b10d-85b86391cc15)

![Screenshot from 2024-09-02 17-58-19](https://github.com/user-attachments/assets/d4b497dd-931c-42b6-890a-aed0b83e7a01)

modify the vsdbabysoc.v file to point to our core clock.

![Screenshot from 2024-09-02 17-59-27](https://github.com/user-attachments/assets/a816bb3c-c851-4ab4-973d-0ee7520fef45)

Next following steps after making the above changes :

```bash
iverilog -o ./pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module/
```
![Screenshot from 2024-09-02 17-31-25](https://github.com/user-attachments/assets/a3cd7c1d-21e7-4835-90de-878a08671b5f)


```bash
./pre_synth_sim.out
```

![Screenshot from 2024-09-02 17-31-25](https://github.com/user-attachments/assets/f7b4afaf-d8a1-4cfe-83af-8f6a819a649c)


3. open GTKwave

![Screenshot from 2024-09-02 17-31-25](https://github.com/user-attachments/assets/dd3d611d-c26a-4002-946a-3040467b6716)


#### waveforms

![Screenshot from 2024-09-02 21-49-53](https://github.com/user-attachments/assets/4933492d-7f0a-4e07-9114-dcfdc7ee82ab)



simulation successfully demonstrates integration of DAC and PLL peripherals with the RISC-V processor, converting digital outputs to analog signals.

</details>

<details>
<summary>LAB 1o: RTL Design using Verilog with Sky130 Technology</summary>

## Day 1 : Introduction to Verilog RTL design and Synthesis.

RTL (Register-Transfer Level) design models synchronous digital circuits by describing the flow of data between hardware registers and the logic operations applied to signals. Verilog is commonly used to create these high-level hardware descriptions. 

To verify the design, simulation tools like Verilog are used. Simulators work by monitoring input signals, and when changes occur, the corresponding output signals are evaluated based on the design. The key to testing the design is the creation of a test bench, which applies stimulus (test vectors) to the circuit to ensure it behaves according to the specifications.

After simulation, the results are saved in a VCD (Value Change Dump) file, which captures how the signals change over time. This VCD file can be loaded into GTKWave, a waveform viewer. GTKWave helps analyze signal interactions, timing relationships, and overall behavior of the circuit. By inspecting the waveforms, designers can debug and validate that the circuit is functioning as intended and meets the required design specifications.

A test bench is a simulation environment used to verify the functionality of a digital circuit design. It applies input stimulus (test vectors) to the design and monitors the outputs to ensure the circuit behaves as expected. Test benches are written in the same hardware description language (e.g., Verilog) and are crucial for validating and debugging RTL designs.

![Screenshot from 2024-10-21 12-17-11](https://github.com/user-attachments/assets/fd40d55c-7ed5-4783-aa0d-98268317f564)

![Screenshot from 2024-10-21 12-17-28](https://github.com/user-attachments/assets/e1c8a5dd-b009-47cd-8d8c-f1aee9f97b53)

#### Lab

```bash
mkdir ASIC
cd ASIC
```

```bash
git clone https://github.com/kunalg123/vsdflow.git
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
```

![Screenshot from 2024-10-20 17-47-54](https://github.com/user-attachments/assets/9e3169be-0f44-4fcd-a70f-6c68520bd93c)

My_Lib contains all the necessary library files.

![Screenshot from 2024-10-21 12-24-28](https://github.com/user-attachments/assets/3ee5efab-776f-454f-87ed-306f9dd798bc)


Simulation using iverilog simulator - 2:1 multiplexer rtl design

Compile the verilog and testbench file usin the command :
```bash
iverilog good_mux.v tb_good_mux.v
```
![Screenshot from 2024-10-21 12-25-22](https://github.com/user-attachments/assets/af82c079-6528-4cd0-ab41-6ab2727203ff)

GTK wave output

```bash
./a.out
```
```bash
gtkwave tb_good_mux.vcd
```

![Screenshot from 2024-10-21 12-27-44](https://github.com/user-attachments/assets/295aa86f-d883-4900-8567-120e478d651a)

To see the contents of the file :

```bash
vim tb_good_mux.v -o good_mux.v
```
![Screenshot from 2024-10-20 20-06-16](https://github.com/user-attachments/assets/881ebc82-be68-4e8d-8bd1-3a97ec755564)

#### Yosys

![Screenshot from 2024-10-21 12-30-56](https://github.com/user-attachments/assets/1459dde3-4425-4d21-b456-481238c9ee85)

![Screenshot from 2024-10-21 12-31-14](https://github.com/user-attachments/assets/6c394c4c-b0da-4c34-9c9c-f41bcd82c25c)

Synthesis is the process of transforming an RTL (Register-Transfer Level) design, described in a hardware description language like Verilog, into a gate-level representation. This gate-level representation is a netlist, which consists of interconnected logic gates. The steps in the synthesis process are as follows:

1. **RTL to Gate-Level Translation**: The RTL design is translated into a netlist, which is a collection of logic gates and their connections. The netlist retains the same primary inputs and outputs as the RTL design, ensuring that the same test bench used for simulation can be reused to verify the synthesized design.

2. **Liberty (.lib) File**: This file contains a library of logical modules, which include basic gates like AND, OR, and NOT. Each gate may have multiple variations (e.g., 2-input, 3-input gates) with different performance characteristics—some are optimized for speed (fast cells), while others for area or power efficiency (slow cells). The selection of these cells is crucial during synthesis, as it impacts the overall performance, power consumption, and area of the final design.

   - **Fast Cells**: Used when high performance is required, but they consume more power and area. They help reduce cell delay by charging or discharging circuit capacitance faster using wider transistors.
   
   - **Slow Cells**: Used to address hold time violations and reduce power and area usage. However, they introduce higher delays due to slower capacitance charging.

3. **Constraints**: A key aspect of synthesis is the use of constraint files. These files guide the synthesis tool in selecting the optimal mix of fast and slow cells based on the desired trade-offs between performance, power consumption, and area. Constraints help ensure that the synthesized design meets the specific timing and performance requirements.

#### Below commands are for synthesis

```bash
Yosys
```

![Screenshot from 2024-10-21 23-09-43](https://github.com/user-attachments/assets/f3d88c27-302f-4d40-b8a2-159ab3709862)


```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
![Screenshot from 2024-10-21 23-09-43](https://github.com/user-attachments/assets/f3d88c27-302f-4d40-b8a2-159ab3709862)


```bash
read_verilog good_mux.v
```
![Screenshot from 2024-10-21 23-09-43](https://github.com/user-attachments/assets/f3d88c27-302f-4d40-b8a2-159ab3709862)

```bash
synth -top good_mux
```
![Screenshot from 2024-10-20 20-29-28](https://github.com/user-attachments/assets/f85ae28f-4e65-4d86-b931-774979555a65)

![Screenshot from 2024-10-20 20-29-40](https://github.com/user-attachments/assets/ff405a84-30b2-43b6-8505-712e646f6344)

```bash
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
![Screenshot from 2024-10-20 20-30-17](https://github.com/user-attachments/assets/ab0b9972-8602-4b75-9ae5-46edf1818f5f)

```bash
show
```
![Screenshot from 2024-10-21 23-06-04](https://github.com/user-attachments/assets/3f2ec591-6226-4128-828f-0bbe4bb20e86)


```bash
write_verilog -noattr good_mux_netlist.v
```
```bash
!vim good_mux_netlist.v
```
![Screenshot from 2024-10-21 13-07-59](https://github.com/user-attachments/assets/c421d108-1fcd-4713-addd-24d61c15b3a2)


## Day 2 : Timing libs, hierarchical vs flat synthesis and efficient flop coding styles .

Run the following commands :

``` bash
vim sky130_fd_sc_hd__tt_025C_1v80.lib
```
![Screenshot from 2024-10-21 23-26-33](https://github.com/user-attachments/assets/f606929d-142b-4e13-911e-023c754a8b05)

Cell library

![Screenshot from 2024-10-21 23-31-05](https://github.com/user-attachments/assets/5c9b83dc-75b7-422c-bd22-38df9b36fd9e)

![Screenshot from 2024-10-21 23-32-52](https://github.com/user-attachments/assets/6ca4b253-fafb-4c14-9317-200271fdcee5)

![Screenshot from 2024-10-21 23-33-35](https://github.com/user-attachments/assets/37c8796f-9863-4949-8d56-0b6b9979759c)

![Screenshot from 2024-10-21 23-33-48](https://github.com/user-attachments/assets/16fbe852-1702-4088-8a8e-cf8084cd5d7f)


The timing data of standard cells is provided in the liberty format. Every .lib file will provide timing, power, noise, area information for a single corner ie process,voltage, temperature etc.

Library : general information common to all cells in the library.
Cell : specific information about each standard cell.
Pin : Timing, power, capacitance, leakage functionality etc characteristics for each pin in each cell.

and2_0 -- takes the least area, more delay and low power.

and2_1 -- takes more area, less delay and high power.

and2_2 -- takes the largest area, larger delay and highest power.


## Hierarchial synthesis vs Flat synthesis

#### Hierarchial synthesis

Hierarchical synthesis breaks a complex design into smaller sub-modules, which are synthesized individually to generate gate-level netlists before being integrated. This method improves organization, supports module reuse, and allows for incremental changes without affecting the entire design. In contrast, flat synthesis treats the entire design as one unit during synthesis, producing a single netlist without considering hierarchy. Although flat synthesis can optimize some designs, it lacks modular structure, making it harder to manage, analyze, and modify.

```bash
vim multiple_modules.v
```

![Screenshot from 2024-10-21 23-39-55](https://github.com/user-attachments/assets/5e51b660-6040-4225-b516-bbc054983a7e)

![Screenshot from 2024-10-21 23-39-17](https://github.com/user-attachments/assets/5d515950-512d-42b3-ba35-b429b5b6ea1d)

To perform hierarchical synthesis on the `multiple_modules.v` file use the following commands:

```bash
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog multiple_modules.v

synth -top multiple_modules

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show multiple_modules

write_verilog -noattr multiple_modules_hier.v

!vim multiple_modules_hier.v
```
![Screenshot from 2024-10-21 23-42-01](https://github.com/user-attachments/assets/07dfdab3-0b5c-4cf8-bd3e-386568c2cf7d)

![Screenshot from 2024-10-21 23-42-17](https://github.com/user-attachments/assets/7a45d8ad-edcf-4e0c-8631-1eb3989a541e)

![Screenshot from 2024-10-21 23-42-29](https://github.com/user-attachments/assets/efb4b9b1-4f49-4a9e-b218-169509c52225)

Realization of the Logic

![Screenshot from 2024-10-21 23-42-56](https://github.com/user-attachments/assets/a4f43f6d-7bc3-4a3e-b01b-5375059f7bce)


![Screenshot from 2024-10-21 23-43-26](https://github.com/user-attachments/assets/a4ca4ba0-7df5-48da-aa7e-9c3f3ea3aa9b)



#### Flat synthesis

To perform flat synthesis on the `multiple_modules.v` file type the following commands:


```bash
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog multiple_modules.v

synth -top multiple_modules

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

flatten

show

write_verilog -noattr multiple_modules_flat.v

!vim multiple_modules_flat.v
```

Realisation of logic

![Screenshot from 2024-10-21 23-49-09](https://github.com/user-attachments/assets/b6080037-3a58-4951-a52b-2b0d523a49e5)

netlist file

![Screenshot from 2024-10-21 23-50-24](https://github.com/user-attachments/assets/10479dab-9ac9-4db1-9834-0fd59a98c469)


#### Sub Module Level Synthesis

This approach is ideal when a design includes multiple instances of the same module. The module is synthesized once and then replicated as needed, with the instances being connected in the top-level module. It leverages the divide-and-conquer strategy, simplifying design management and optimizing synthesis by reusing the same synthesized module.

```bash
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog multiple_modules.v

synth -top sub_module1

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show
```

![Screenshot from 2024-10-21 23-58-00](https://github.com/user-attachments/assets/f839419c-1fbc-47c7-8bb8-4945f61a96de)

![Screenshot from 2024-10-21 23-58-44](https://github.com/user-attachments/assets/6d431174-a5a7-4aa8-ba73-60b62bd1b185)



#### Flop coding styles and optimization

Flip-flops play a crucial role in sequential logic circuits, and their design and synthesis are explored here. To prevent glitches in digital circuits, flip-flops store intermediate values, ensuring that the inputs to combinational circuits stay stable until the clock edge. This prevents glitches and ensures proper operation by maintaining signal stability.

Asynchronous Reset/set:

Verilog Code for Asynchronous Reset:

![Screenshot from 2024-10-22 00-08-57](https://github.com/user-attachments/assets/e7c3f6cb-2966-4c74-b357-5b4a015bdecb)

Verilog Code for Asynchronous Reset:

![Screenshot from 2024-10-22 00-09-32](https://github.com/user-attachments/assets/55857a08-73dc-403b-8579-60ccdd96b508)

Synchronous Reset:

![Screenshot from 2024-10-22 00-10-17](https://github.com/user-attachments/assets/29191662-d562-49e5-ae21-557686cda7c5)


FLIP FLOP SIMULATION

```bash
iverilog dff_asyncres.v tb_dff_asyncres.v 

ls

./a.out

gtkwave tb_dff_asyncres.vcd
```
![Screenshot from 2024-10-22 00-12-19](https://github.com/user-attachments/assets/ba0aff7f-9e68-4314-862a-844661a12e99)

GTK WAVE OF ASYNCHRONOUS RESET

![Screenshot from 2024-10-22 00-16-50](https://github.com/user-attachments/assets/8d4b162a-6495-45f8-9cc7-37303f426c04)

GTK wave of Asyncrhonous set

![Screenshot from 2024-10-22 00-21-58](https://github.com/user-attachments/assets/1c68252c-c7ea-4276-88da-b0bd7df494dd)


GTK wave of synchronous reset

![Screenshot from 2024-10-22 00-24-17](https://github.com/user-attachments/assets/8ac66a8c-c65a-4f8d-bdd7-68f27336573e)


FLIP FLOP SYNTHESIS

```bash

yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog dff_asyncres.v

synth -top dff_asyncres

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show
```

Statistics of D FLipflop with Asynchronous Reset

![Screenshot from 2024-10-22 00-25-54](https://github.com/user-attachments/assets/2bee8f72-8cf6-44b6-b995-8a2da8bca2e7)


Realization of Logic

![Screenshot from 2024-10-22 00-26-36](https://github.com/user-attachments/assets/7ea059e3-6345-4f87-9595-04f4462b9486)

We initially designed a flip-flop with an active-high reset, but it was behaving as if it had an active-low reset. To correct this, the tool inserted an inverter, making the reset signal `!(!(reset))`, which effectively returned it to an active-high reset. Ultimately, we ended up with the intended active-high reset for the flip-flop.


Statistics of D FLipflop with Asynchronous set

```bash

yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog dff_async_set.v

synth -top dff_async_set

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show
```

realisation of logic

![Screenshot from 2024-10-22 00-33-37](https://github.com/user-attachments/assets/c3b41f46-74c8-4c78-989b-f9dcc6b3cd49)


```bash

yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog dff_syncres.v

synth -top dff_syncres

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show
```
![Screenshot from 2024-10-22 00-33-51](https://github.com/user-attachments/assets/ee825f56-e868-4fce-9521-a46d7254875b)

#### Optimizations

Example 1: mult_2.v

verilog code

![Screenshot from 2024-10-22 00-40-58](https://github.com/user-attachments/assets/96a03b5e-1f38-411e-bab8-4b7b02117621)

Truth table

![Screenshot from 2024-10-22 00-42-24](https://github.com/user-attachments/assets/e53f3ed4-4bdf-4181-aa5f-63d1004dda45)


Multiplying a number by 2 doesn't require additional hardware; it can be achieved by appending zeros to the least significant bits (LSBs) while passing the remaining input bits directly to the output. This is done by grounding the LSBs and correctly wiring the input to the output.

```bash
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog mult_2.v

synth -top mul2

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show 

write_verilog -noattr mult_2_net.v

!vim mult_2_net.v
```

Statistics

![Screenshot from 2024-10-22 00-49-13](https://github.com/user-attachments/assets/7d57c81d-a628-4f17-9069-1290e67525a6)


Realisation of logic

![Screenshot from 2024-10-22 00-49-33](https://github.com/user-attachments/assets/8180839f-8469-4af1-b936-3f0cc08c4393)


Netlist

![Screenshot from 2024-10-22 00-49-52](https://github.com/user-attachments/assets/d5643d4d-ed8c-4b19-9c04-4109a2f1ea73)


Example 2: mult_8.v

verilog code

![Screenshot from 2024-10-22 00-53-18](https://github.com/user-attachments/assets/08aa80ca-b2f4-4817-92f9-47abf9da34a5)


Logic behaviour


![Screenshot from 2024-10-22 00-53-40](https://github.com/user-attachments/assets/b1467bb3-8efc-4a37-97b9-5e20da48bbff)


In this design, the 3-bit input number "a" is multiplied by 9, represented as (a9), which can be rewritten as (a8) + a. The term (a8) is equivalent to left-shifting "a" by three bits. Given a = a2 a1 a0, shifting left by three bits gives (a8) = a2 a1 a0 0 0 0. Therefore, (a9) = (a8) + a = a2 a1 a0 a2 a1 a0, effectively doubling "a" into a 6-bit number. Since this results in simply repeating the bits, no additional hardware is required. The synthesized netlist for this design is shown below.

```bash
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog mult_8.v

synth -top mult8

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr mult_8_net.v

!vim mult_8_net.v
```
Statistics

![Screenshot from 2024-10-22 00-55-25](https://github.com/user-attachments/assets/52f653dd-1276-4468-9a35-92fcce5eb227)



Realisation of logic

![Screenshot from 2024-10-22 00-55-59](https://github.com/user-attachments/assets/24c5895a-3711-4e7d-9730-3c1d7a513ba6)

netlist

![Screenshot from 2024-10-22 00-57-19](https://github.com/user-attachments/assets/6b01ae2f-a776-4022-91fa-e7f6b75a85a1)


## Day 3  : Combinational and Sequential Optimizations 

There are two main types of optimizations: combinational and sequential, both aimed at creating designs that are efficient in terms of area, power, and performance. Combinational optimization involves techniques such as constant propagation, which is a direct optimization method, and Boolean logic optimization, which can be accomplished using tools like Karnaugh Maps (K-Maps) or the Quine-McCluskey method.

Constant Propagation:

Consider the below circuit:

![Screenshot from 2024-10-22 01-14-38](https://github.com/user-attachments/assets/fae532d2-5429-4939-8718-0724d2f1a363)

The top circuit uses 6 transistors (3 NMOS and 3 PMOS), while the bottom circuit uses 2 transistors (1 NMOS and 1 PMOS) when input A is set to zero, turning the logic into an inverter.

Boolean Logic Optimisation:

verilog code:

```bash
assign y = a?(b?c:(c?a:0)):(!c)
```
The ternary operator (?:) will make the circuit behave like a mux upon synthesis as shown below.

![Screenshot from 2024-10-22 01-16-01](https://github.com/user-attachments/assets/66a5e4ae-5cba-44f0-9808-a57ebe236ca2)

The circuit can be optimised as follows:

![Screenshot from 2024-10-22 01-16-35](https://github.com/user-attachments/assets/fbb228d9-656d-4f21-9e1b-0fd0f1e4ffc4)


Example 1:

verilog code :

![Screenshot from 2024-10-22 01-18-20](https://github.com/user-attachments/assets/872e2131-9766-4414-8360-acf3e479aa66)

A multiplexer and since one of the inputs of the multiplexer is always connected to the ground it will infer an AND gate on optimisation

```bash
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog opt_check.v

synth -top opt_check

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr opt_check_net.v

!vim opt_check_net.v
```

![Screenshot from 2024-10-22 01-23-08](https://github.com/user-attachments/assets/a2e82dbd-826c-4af1-b5ec-56265817d569)


![Screenshot from 2024-10-22 01-23-51](https://github.com/user-attachments/assets/3b40df22-bf7f-4878-8712-80d3acd53901)


Example 2:

Verilog code:

Since one of the inputs of the multiplexer is always connected to the logic 1 it will infer an OR gate on optimisation.The OR gate will be NAND implementation since NOR gate has stacked pmos while NAND implementation has stacked nmos.


![Screenshot from 2024-10-22 01-25-19](https://github.com/user-attachments/assets/ef2f840a-86ca-469c-942d-6faf3910bc2e)

```bash
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog opt_check2.v

synth -top opt_check2

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr opt_check2_net.v

!vim opt_check2_net.v
```
![Screenshot from 2024-10-22 01-28-13](https://github.com/user-attachments/assets/7830bcd7-c3fc-4e57-a1f5-ac08990b9dc6)

![Screenshot from 2024-10-22 01-28-45](https://github.com/user-attachments/assets/df9af0b1-2f4c-46d0-9ee3-d1ddf2122819)


Example 3:

Verilog code:

![Screenshot from 2024-10-22 01-29-48](https://github.com/user-attachments/assets/103e7ef5-cb5a-45ff-8db6-e056bb1ecb21)


On optimisation the above design becomes a 3 input AND gate

```bash
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog opt_check3.v

synth -top opt_check3

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr opt_check3_net.v

!vim opt_check3_net.v
```

![Screenshot from 2024-10-22 01-32-16](https://github.com/user-attachments/assets/b30a0b7b-ef6a-4536-ac11-9ce5a8d26618)

![Screenshot from 2024-10-22 01-32-45](https://github.com/user-attachments/assets/a55cbe48-e3cd-4ad5-8d11-a67a5e0cc5de)



Example 4:

Verilog code:

![Screenshot from 2024-10-22 01-33-46](https://github.com/user-attachments/assets/67a72478-433c-4799-9289-64dc1f802802)


On optimisation the above design becomes a 2 input XNOR gate.

```bash
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog opt_check4.v

synth -top opt_check4

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr opt_check4_net.v

!vim opt_check4_net.v
```

![Screenshot from 2024-10-22 01-35-27](https://github.com/user-attachments/assets/a0315ed3-84c2-425b-814c-8cc6ce5a978a)

![Screenshot from 2024-10-22 01-35-49](https://github.com/user-attachments/assets/68ee9d28-40b4-45a9-970d-9339f6c2e33e)


Example 5:

Verilog code:

![Screenshot from 2024-10-22 01-38-03](https://github.com/user-attachments/assets/a8f3169d-5bb1-43ee-a0d7-f138b342e6e5)


```bash
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog multiple_module_opt.v

synth -top multiple_module_opt

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

flatten

show

write_verilog -noattr multiple_module_opt_net.v

!vim multiple_module_opt_net.v
```

![Screenshot from 2024-10-22 01-39-39](https://github.com/user-attachments/assets/946f8186-fcee-4628-a8ca-333905ccb2af)

![Screenshot from 2024-10-22 01-39-57](https://github.com/user-attachments/assets/227dcb3d-37ac-4169-b79c-f9a95681be26)

![Screenshot from 2024-10-22 01-40-53](https://github.com/user-attachments/assets/f4a87c96-ac6c-4bf8-a3c0-33d9e9334c6b)


```bash
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog multiple_module_opt2.v

synth -top multiple_module_opt2

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

flatten

show

write_verilog -noattr multiple_module_opt2_net.v

!vim multiple_module_opt_net2.v
```

![Screenshot from 2024-10-22 01-42-25](https://github.com/user-attachments/assets/194073a5-ca25-4c04-8a67-864dccdc5b30)


![Screenshot from 2024-10-22 01-43-11](https://github.com/user-attachments/assets/d3a88344-ad8f-49cd-b14e-aaf664af1b39)


Sequential Logic Optimizations

Example 1:

Verilog code:

![Screenshot from 2024-10-22 01-47-25](https://github.com/user-attachments/assets/2823a661-26c3-4801-bc7d-af7f3aab53cf)

```bash
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

  read_verilog dff_const1.v

synth -top dff_const1

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr dff_const1_net.v

!vim dff_const1_net.v
```

![Screenshot from 2024-10-22 01-49-10](https://github.com/user-attachments/assets/d24d6b92-c211-4783-95e8-706c4bbafd20)

![Screenshot from 2024-10-22 01-49-38](https://github.com/user-attachments/assets/80af144a-7c65-498a-bbb8-53e0ad47962b)


```bash
iverilog dff_const1.v tb_dff_const1.v

./a.out

gtkwave tb_dff_const1.vcd
```
![Screenshot from 2024-10-22 01-52-12](https://github.com/user-attachments/assets/8c5297c5-a18c-4752-83d4-7e6f33618602)


Example 2:

Verilog code:

![Screenshot from 2024-10-22 01-53-07](https://github.com/user-attachments/assets/61a1631a-f17b-48f6-94a3-d88b776635d8)

```bash
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog dff_const2.v

synth -top dff_const2

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr dff_const2_net.v

!vim dff_const2_net.v
```
![Screenshot from 2024-10-22 01-55-05](https://github.com/user-attachments/assets/23085b25-5396-429c-8d4d-f4cb8e6d3eff)

![Screenshot from 2024-10-22 01-55-26](https://github.com/user-attachments/assets/b1862c62-5b24-482f-8dbc-3d4dcf29dc1d)

```bash
iverilog dff_const2.v tb_dff_const2.v

./a.out

gtkwave tb_dff_const2.vcd
```

![Screenshot from 2024-10-22 01-57-05](https://github.com/user-attachments/assets/91c926b7-e564-4650-9a3f-195eebe84ae7)

Example 3:

Verilog code:

![Screenshot from 2024-10-22 01-58-20](https://github.com/user-attachments/assets/9b50f73b-02eb-4079-85a8-18215b505e9b)


```bash
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog dff_const3.v

synth -top dff_const3

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr dff_const3_net.v
```

![Screenshot from 2024-10-22 01-59-50](https://github.com/user-attachments/assets/39178b26-26d4-4b4d-89c2-7f0f2342a5cb)

![Screenshot from 2024-10-22 02-00-50](https://github.com/user-attachments/assets/1479ee6a-11e1-41b0-b490-450e29879e6a)

```bash
iverilog dff_const3.v tb_dff_const3.v

./a.out

gtkwave tb_dff_const3.vcd
```

![Screenshot from 2024-10-22 02-02-44](https://github.com/user-attachments/assets/958d0fa3-13a1-4b00-a948-7ed2cd98fb18)

Example 4:

Verilog code:

![Screenshot from 2024-10-22 02-03-22](https://github.com/user-attachments/assets/186589a1-3645-4bda-b2f1-14c8ec45b364)

```bash
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog dff_const4.v

synth -top dff_const4

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr dff_const4_net.v
```

![Screenshot from 2024-10-22 02-04-43](https://github.com/user-attachments/assets/b5a5fe0a-ae77-4447-9bfa-590ba1fc3d31)

![Screenshot from 2024-10-22 02-05-05](https://github.com/user-attachments/assets/1bad7e5e-b7ab-4110-ba1a-453b30304b5b)



```bash
iverilog dff_const4.v tb_dff_const4.v

./a.out

gtkwave tb_dff_const4.vcd
```
![Screenshot from 2024-10-22 02-06-37](https://github.com/user-attachments/assets/3e88e7f2-26b3-41f3-b89f-e2e2deeab8f1)


Example 5:

Verilog code:

![Screenshot from 2024-10-22 02-07-43](https://github.com/user-attachments/assets/882cc4e8-d190-4865-9271-3fa5004d13b8)

```bash
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog dff_const5.v

synth -top dff_const5

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr dff_const5_net.v
```
![Screenshot from 2024-10-22 02-08-53](https://github.com/user-attachments/assets/c15d6597-bf08-4585-a2f5-1404c19f4a1f)

![Screenshot from 2024-10-22 02-09-35](https://github.com/user-attachments/assets/69858709-ca81-41c8-af99-7b2c819283fd)


```bash
iverilog dff_const5.v tb_dff_const5.v

./a.out

gtkwave tb_dff_const5.vcd
```

![Screenshot from 2024-10-22 02-24-38](https://github.com/user-attachments/assets/7c031029-d9e2-4916-96d7-dea8f142d5be)

Sequential Logic Optimizations for unused outputs

Example 1:

Verilog code:

![Screenshot from 2024-10-22 02-25-48](https://github.com/user-attachments/assets/70a95308-6057-4add-8f84-6174c8eacdf0)


```bash
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog counter_opt.v

synth -top counter_opt

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr counter_opt_net.v
```

![Screenshot from 2024-10-22 02-28-19](https://github.com/user-attachments/assets/a28b3dab-5b21-48c2-ab0c-2372a722e845)


![Screenshot from 2024-10-22 02-29-12](https://github.com/user-attachments/assets/d3f5c57b-01d3-4617-87ff-be2cf59fcf6d)

```bash
iverilog counter_opt.v tb_counter_opt.v

./a.out

gtkwave tb_counter_opt.vcd
```
![Screenshot from 2024-10-22 02-30-35](https://github.com/user-attachments/assets/babad388-92dd-47fc-a84e-8e44c1c2a47c)


Modified counter logic:

Verilog code:

![Screenshot from 2024-10-22 02-36-23](https://github.com/user-attachments/assets/3892632f-f1a6-4f3e-8b61-aec1be89a6db)


```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog counter_opt2.v
synth -top counter_opt2
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr counter_opt_net.v
```

![Screenshot from 2024-10-22 02-38-06](https://github.com/user-attachments/assets/64ad52bd-61ac-4518-9934-45c4fddbdd54)

![Screenshot from 2024-10-22 02-39-50](https://github.com/user-attachments/assets/4fe68fbc-e282-4f7d-a501-2c86837d221f)


```bash
iverilog counter_opt2.v tb_counter_opt.v

./a.out

gtkwave tb_counter_opt.vcd
```

![Screenshot from 2024-10-22 02-46-08](https://github.com/user-attachments/assets/092eea60-cf5b-406b-9ca6-adfee5b4e667)




## Day 4 : GLS, blocking vs non-blocking and Synthesis-Simulation mismatch.

Introduction to GLS and Synthesis-Simulation mismatch :

Gate Level Simulation (GLS) plays a crucial role in verifying digital circuits by simulating the synthesized netlist, which is a lower-level representation of the design. Using a testbench, GLS assesses the logical accuracy and timing behavior of the circuit. By comparing the simulation outputs to the expected results, GLS helps ensure that the synthesis process has not introduced errors and that the design meets its performance specifications.

Sensitivity lists are essential for maintaining correct circuit behavior. An incomplete sensitivity list can lead to unintended latches. Additionally, blocking and non-blocking assignments in always blocks function differently, and improper use of blocking assignments can inadvertently create latches, resulting in discrepancies between synthesis and simulation. Therefore, it is vital to thoroughly review the circuit behavior and ensure that the sensitivity lists and assignments align with the intended functionality.

![Screenshot from 2024-10-22 02-22-39](https://github.com/user-attachments/assets/45b981d4-65d2-4b50-9876-c31c57d7a116)


GLS Simulation

Example 1:

Verilog code:

![Screenshot from 2024-10-22 02-48-56](https://github.com/user-attachments/assets/2c2a90e3-dab5-4389-bdde-ce689dcf9e7a)


```bash
iverilog ternary_operator_mux.v tb_ternary_operator_mux.v

./a.out

gtkwave tb_ternary_operator_mux.vcd
```

![Screenshot from 2024-10-22 02-50-18](https://github.com/user-attachments/assets/56cd8a98-5a7d-43e0-9df0-0fb398bc1a08)


```bash
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog ternary_operator_mux.v

synth -top ternary_operator_mux

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr ternary_operator_mux_net.v
```

![Screenshot from 2024-10-22 02-52-19](https://github.com/user-attachments/assets/2c12c80d-1e94-4329-bc19-0c3d03cf894e)


![Screenshot from 2024-10-22 02-53-00](https://github.com/user-attachments/assets/8dec6ddd-34af-469f-af63-4e3990609dc2)


GLS:

```bash
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd
```

![Screenshot from 2024-10-22 02-57-52](https://github.com/user-attachments/assets/ae4ac0d9-7faa-48af-babe-935552b4cf66)


no mismatch between the waveforms before and after synthesis

Example 2:

Verilog code:

![Screenshot from 2024-10-22 03-00-31](https://github.com/user-attachments/assets/6b856f3f-d1d2-485b-9d1c-3c222014bd93)


```bash
iverilog bad_mux.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```

![Screenshot from 2024-10-22 03-02-04](https://github.com/user-attachments/assets/57cb46ef-aa93-4fcf-8d25-09168c4170c1)


```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
read_verilog bad_mux.v
synth -top bad_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
show
write_verilog -noattr bad_mux_net.v
```

![Screenshot from 2024-10-22 03-03-13](https://github.com/user-attachments/assets/f0701e21-7e1d-46ba-9779-fcdbec38c156)


![Screenshot from 2024-10-22 03-03-45](https://github.com/user-attachments/assets/638a7771-359b-4ef4-bb3a-29f1901f8dbd)

GLS:

```bash
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_mux_net.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```

![Screenshot from 2024-10-22 03-05-16](https://github.com/user-attachments/assets/365e86fa-80c5-4419-a4d7-3ceb2dcaf383)



There is a synthesis and simulation mismatch. 
While performing synthesis yosys has corrected the sensitivity list error.


Labs on Synthesis-Simulation mismatch for blocking statements

Verilog code:

![Screenshot from 2024-10-22 03-06-27](https://github.com/user-attachments/assets/782aef51-da02-42f2-9701-5302c3caa8a5)


```bash
iverilog blocking_caveat.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```

![Screenshot from 2024-10-22 03-08-15](https://github.com/user-attachments/assets/248f9f80-fc79-48e6-8164-884b3f3e4270)


```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
read_verilog blocking_caveat.v
synth -top blocking_caveat
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
show
write_verilog -noattr blocking_caveat_net.v
```

![Screenshot from 2024-10-22 03-09-29](https://github.com/user-attachments/assets/8eea33d8-a94c-4776-9e30-e11de31ef7ad)


![Screenshot from 2024-10-22 03-09-57](https://github.com/user-attachments/assets/594c33a0-b483-4c39-8d0e-2dbab42c878e)


GLS

```bash
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v blocking_caveat_net.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```


![Screenshot from 2024-10-22 03-11-58](https://github.com/user-attachments/assets/8aac684c-8df7-4625-a9be-285f31ae60b5)


There is a synthesis and simulation mismatch. While performing synthesis yosys has corrected the latch error.


</details>


<details>
<summary>LAB 11: Synthesize RISC-V and Compare Output with Functional Simulations</summary>

## Synthesizing RISC-V and comparing output with functional (RTL) simulation :

Copy the src folder from the BabySoC folder to the sky130RTLDesignAndSynthesisWorkshop folder in the VLSI folder from previous lab.

go to the following directory:

```bash
cd /home/yash/VLSI/sky130RTLDesignAndSynthesisWorkshop/src/module

```

Synthesis : 

Run the following commands

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog clk_gate.v
read_verilog rvmyth.v
synth -top rvmyth
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog -noattr rvmyth.v
!gedit rvmyth.v
```

snapshots :

![Screenshot from 2024-10-24 03-52-06](https://github.com/user-attachments/assets/3719ba07-a824-4ae7-a877-e651739ef3c8)

![Screenshot from 2024-10-24 03-52-21](https://github.com/user-attachments/assets/f3d55a0a-83f4-4527-8c84-e2f581132667)

![Screenshot from 2024-10-24 03-52-42](https://github.com/user-attachments/assets/98b9f39d-398e-4871-8ebd-f5fcf3d32a3e)


```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_liberty -lib ../lib/avsddac.lib
read_liberty -lib ../lib/avsdpll.lib  
read_verilog vsdbabysoc.v
read_verilog rvmyth.v
read_verilog clk_gate.v 
synth -top vsdbabysoc
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
show
write_verilog -noattr vsdbabysoc.synth.v
```

![Screenshot from 2024-10-24 04-07-52](https://github.com/user-attachments/assets/f177540a-4d29-41f0-8b6a-7d510098dcdb)

![Screenshot from 2024-10-24 04-08-17](https://github.com/user-attachments/assets/c8a7c27b-0b4f-4b01-bc63-eab5e8116619)


![Screenshot from 2024-10-24 04-13-36](https://github.com/user-attachments/assets/31ea97a0-0fa3-4163-98d8-80502cb9949d)



![Screenshot from 2024-10-24 04-12-00](https://github.com/user-attachments/assets/41c83782-cf68-4fe9-8490-b4ace042a8b1)




observing the output waveform : 

run the following commands :

```bash
iverilog ../../my_lib/verilog_model/primitives.v ../../my_lib/verilog_model/sky130_fd_sc_hd.v rvmyth.v testbench.v vsdbabysoc.v avsddac.v avsdpll.v clk_gate.v
./a.out
gtkwave dump.vcd
```
![Screenshot from 2024-10-24 03-55-26](https://github.com/user-attachments/assets/d3c6ace0-0ee8-4151-be74-7eed2227b957)

![Screenshot from 2024-10-24 03-55-36](https://github.com/user-attachments/assets/94465caf-cd49-400c-a018-4c8e30389caa)

![Screenshot from 2024-10-24 03-56-01](https://github.com/user-attachments/assets/5d0faa4e-00a3-4dad-a100-b40ce915b859)



Functional Simulations

```bash
iverilog -o ./pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module/
./pre_synth_sim.out
gtkwave pre_synth_sim.vcd
```

![Screenshot from 2024-10-24 04-03-00](https://github.com/user-attachments/assets/3194ec1b-3a80-4986-930a-6f76969062f8)

![Screenshot from 2024-10-24 04-04-31](https://github.com/user-attachments/assets/2a94e1be-9b03-4b51-b0b6-777112b6f678)

![Screenshot from 2024-10-24 04-05-11](https://github.com/user-attachments/assets/8a3c5cec-ba7e-427a-9709-1ab02296a56d)

we can see comparing both the outputs are same hence verifying our results.
</details>


<details>
<summary>LAB 12: Post Synthesis Static Timing Analysis using OpenSTA.</summary>

#### Static Timing Analysis (STA)

Static Timing Analysis (STA) is a method used to verify the timing performance of digital circuits without requiring dynamic simulations. By examining data paths from inputs to outputs, STA assesses whether a circuit meets specific timing constraints, accounting for delays through gates and interconnects. It detects setup and hold time violations, ensuring data stability around clock edges, and considers clock properties such as frequency, skew, and jitter. STA assumes worst-case delay conditions to confirm performance under diverse conditions. Tools like Synopsys PrimeTime and Cadence Tempus automate STA, enabling early detection of timing issues to ensure reliable circuit operation at the intended speeds.
STA is vital in digital design for multiple reasons. It verifies that data signals propagate within defined time limits to produce valid outputs and identifies timing violations, ensuring reliable operation of flip-flops and other sequential components. STA also assists in performance optimization by pinpointing critical paths that limit the maximum operating frequency, allowing for early detection of timing issues and reducing the need for costly redesigns. Additionally, STA facilitates balancing performance with power efficiency by analyzing the power impact of clock frequency adjustments. Automated STA tools manage complex designs, accounting for variations in manufacturing, temperature, and voltage to ensure stable performance across conditions.


#### Reg-to-Reg Path

A reg-to-reg path links two sequential elements, such as flip-flops, within a digital circuit. This path is a primary focus in Static Timing Analysis (STA) as it verifies data transfer between registers through combinational logic, crucial for accurate synchronization, especially in pipelined or sequential designs. STA checks setup and hold time requirements along these paths to ensure data stability at the registers. It considers delays in the connecting combinational logic, with critical reg-to-reg paths influencing the circuit’s maximum achievable frequency. For registers in separate clock domains, STA also addresses factors like metastability and synchronization to maintain reliable operation.

#### Clk-to-Reg Path

A clk-to-reg path links a clock signal to a register, enabling the register to operate correctly with each clock edge. This path measures the delay for the clock signal to reach the register from its source, factoring in delays from buffers or routing. During setup analysis, the clk-to-reg path defines the timing requirement for data arrival at the register relative to the clock edge. Clock delay influences data capture timing, and both setup and hold timing are evaluated, especially in the presence of clock jitter or variations. For registers in different clock domains, additional considerations ensure reliable synchronization across domains.

#### STA for synthesized Risc-V core using time period of 11.40 ns.

The contents of VSDBabySoc/src/sdc/vsdbabysoc_synthesis.sdc:

```bash
set PERIOD 11.40

set_units -time ns
create_clock [get_pins {pll/CLK}] -name clk -period $PERIOD
set_clock_uncertainty -setup  [expr $PERIOD * 0.05] [get_clocks clk]
set_clock_transition [expr $PERIOD * 0.05] [get_clocks clk]
set_clock_uncertainty -hold [expr $PERIOD * 0.08] [get_clocks clk]
set_input_transition [expr $PERIOD * 0.08] [get_ports ENb_CP]
set_input_transition [expr $PERIOD * 0.08] [get_ports ENb_VCO]
set_input_transition [expr $PERIOD * 0.08] [get_ports REF]
set_input_transition [expr $PERIOD * 0.08] [get_ports VCO_IN]
set_input_transition [expr $PERIOD * 0.08] [get_ports VREFH]
```


Run the following commands :

```bash
cd VSDBabySoc/src
sta
read_liberty -min ./lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_liberty -min ./lib/avsdpll.lib
read_liberty -min ./lib/avsddac.lib
read_liberty -max ./lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_liberty -max ./lib/avsdpll.lib
read_liberty -max ./lib/avsddac.lib
read_verilog ../output/synth/vsdbabysoc.synth.v
link_design vsdbabysoc
read_sdc ./sdc/vsdbabysoc_synthesis.sdc
report_checks -path_delay min_max -format full_clock_expanded -digits 4
```

Snapshots :

![3n](https://github.com/user-attachments/assets/f2cf1b2e-5f9e-4eb7-bd9f-4232c37c45a3)


Setup time :
![2n](https://github.com/user-attachments/assets/0b87ef63-4aa0-4687-a3bb-0928c2bcb654)

![image](https://github.com/user-attachments/assets/7cc27750-cef6-4c63-bdc2-8f544a240512)


Hold time :

![1n](https://github.com/user-attachments/assets/c7326b93-8d82-4b19-bb18-0c7ead1e6fd9)

![image](https://github.com/user-attachments/assets/e4d22e09-4b68-4bdb-96f2-b37df482b8d3)


</details>


<details>
<summary>LAB 13: Post Synthesis Static Timing Analysis using OpenSTA for all the sky130 lib files.</summary>

sdc file vsdbabysoc_synthesis.sdc:

```bash
set PERIOD 11.4
set_units -time ns
create_clock [get_pins {pll/CLK}] -name clk -period $PERIOD
set_clock_uncertainty -setup  [expr $PERIOD * 0.05] [get_clocks clk]
set_clock_transition [expr $PERIOD * 0.05] [get_clocks clk]
set_clock_uncertainty -hold [expr $PERIOD * 0.08] [get_clocks clk]
set_input_transition [expr $PERIOD * 0.08] [get_ports ENb_CP]
set_input_transition [expr $PERIOD * 0.08] [get_ports ENb_VCO]
set_input_transition [expr $PERIOD * 0.08] [get_ports REF]
set_input_transition [expr $PERIOD * 0.08] [get_ports VCO_IN]
set_input_transition [expr $PERIOD * 0.08] [get_ports VREFH]
```

![Screenshot from 2024-11-04 17-47-18](https://github.com/user-attachments/assets/3e9d779c-a1ef-42ec-9517-9b04020bbd79)


create a sta.tcl file and copy the content below in that file using gedit.

![Screenshot from 2024-11-04 17-48-27](https://github.com/user-attachments/assets/d86b0dbc-ec99-4918-9e8c-b7f6089d88b6)


```bash
set list_of_lib_files(1) "sky130_fd_sc_hd__ff_100C_1v65.lib"
set list_of_lib_files(2) "sky130_fd_sc_hd__ff_100C_1v95.lib"
set list_of_lib_files(3) "sky130_fd_sc_hd__ff_n40C_1v56.lib"
set list_of_lib_files(4) "sky130_fd_sc_hd__ff_n40C_1v65.lib"
set list_of_lib_files(5) "sky130_fd_sc_hd__ff_n40C_1v76.lib"
set list_of_lib_files(6) "sky130_fd_sc_hd__ff_n40C_1v95.lib"
set list_of_lib_files(7) "sky130_fd_sc_hd__ff_n40C_1v95_ccsnoise.lib.part1"
set list_of_lib_files(8) "sky130_fd_sc_hd__ff_n40C_1v95_ccsnoise.lib.part2"
set list_of_lib_files(9) "sky130_fd_sc_hd__ff_n40C_1v95_ccsnoise.lib.part3"
set list_of_lib_files(10) "sky130_fd_sc_hd__ss_100C_1v40.lib"
set list_of_lib_files(11) "sky130_fd_sc_hd__ss_100C_1v60.lib"
set list_of_lib_files(12) "sky130_fd_sc_hd__ss_n40C_1v28.lib"
set list_of_lib_files(13) "sky130_fd_sc_hd__ss_n40C_1v35.lib"
set list_of_lib_files(14) "sky130_fd_sc_hd__ss_n40C_1v40.lib"
set list_of_lib_files(15) "sky130_fd_sc_hd__ss_n40C_1v44.lib"
set list_of_lib_files(16) "sky130_fd_sc_hd__ss_n40C_1v60.lib"
set list_of_lib_files(17) "sky130_fd_sc_hd__ss_n40C_1v60_ccsnoise.lib.part1"
set list_of_lib_files(18) "sky130_fd_sc_hd__ss_n40C_1v60_ccsnoise.lib.part2"
set list_of_lib_files(19) "sky130_fd_sc_hd__ss_n40C_1v60_ccsnoise.lib.part3"
set list_of_lib_files(20) "sky130_fd_sc_hd__ss_n40C_1v76.lib"
set list_of_lib_files(21) "sky130_fd_sc_hd__tt_025C_1v80.lib"
set list_of_lib_files(22) "sky130_fd_sc_hd__tt_100C_1v80.lib"

for {set i 1} {$i <= [array size list_of_lib_files]} {incr i} {
read_liberty ./timing_libs/$list_of_lib_files($i)
read_verilog ../output/synth/vsdbabysoc.synth.v
link_design vsdbabysoc
read_sdc ./sdc/vsdbabysoc_synthesis.sdc
check_setup -verbose
report_checks -path_delay min_max -fields {nets cap slew input_pins fanout} -digits {4} > ./sta_output/min_max_$list_of_lib_files($i).txt

}
```

![Screenshot from 2024-11-04 17-48-55](https://github.com/user-attachments/assets/ef4f47a7-4ede-4533-a224-d3d0b3cbe242)


![Screenshot from 2024-11-04 17-48-17](https://github.com/user-attachments/assets/6246ae2e-39b3-4f3a-9205-67c4fcc95f18)

Run the following commands :

```bash
cd VSDBabySoC/src
sta
source sta_pvt.tcl
```

![Screenshot from 2024-11-04 17-48-55](https://github.com/user-attachments/assets/48baa28f-fe57-487b-b7d7-784e5db5fd1b)

![Screenshot from 2024-11-04 17-49-17](https://github.com/user-attachments/assets/da244e2a-784a-48d3-94d8-b24546b14517)

![Screenshot 2024-11-04 183132](https://github.com/user-attachments/assets/62450e51-99a0-4b9c-aa8a-a83f57679f75)


 Put the values in excel and plot the graphs :

 ![Screenshot 2024-11-04 182520](https://github.com/user-attachments/assets/22730cb5-8909-4ca1-9189-97b055060801)

![Screenshot 2024-11-04 182736](https://github.com/user-attachments/assets/68a92235-41dc-480d-8144-270b46e81720)


![Screenshot 2024-11-04 182751](https://github.com/user-attachments/assets/53f2d688-0aed-48cf-a71c-91de767dc221)


</details>

