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
<summary> LAB 7 : Digital logic with TL-verilog and Makerchip </summary>

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

   // /====================\
   // | Sum 1 to 9 Program |
   // \====================/
   //
   // Program for MYTH Workshop to test RV32I
   // Add 1,2,3,...,9 (in that order).
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
   
   // Optional:
   // m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;
         $clk_yas = *clk;
         $pc[31:0] = >>1$reset ? 32'b0 :
            >>1$taken_branch ? >>1$br_target_pc :
            >>1$pc + 32'd4;
         $imem_rd_en = >>1$reset ? 0 : 1;
         $imem_rd_addr[31:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
      @1
         $instr[31:0] = $imem_rd_data[31:0];
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
              $instr[6:2] ==? 5'b001x0 ||
              $instr[6:2] ==? 5'b11001;
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
              $instr[6:2] ==? 5'b011x0 ||
              $instr[6:2] ==? 5'b10100;
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
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
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index1[4:0] = $rs1;
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
                         $is_bgeu ? ($src1_value >= $src2_value):1'b0;

         $br_target_pc[31:0] = $pc +$imm;

      // YOUR CODE HERE
      
      
      // ...

      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

   
   // Assert these to end simulation (before Makerchip cycle limit).
   *passed = *cyc_cnt > 40;
   *passed = |cpu/xreg[10]>>5$value == (1+2+3+4+5+6+7+8+9) ;
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

<img width="556" alt="image" src="https://github.com/user-attachments/assets/a4e1113f-0287-4b6f-919e-e8f9b05cda39">


</details>

