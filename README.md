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
![Screenshot from 2024-11-04 17-48-17](https://github.com/user-attachments/assets/6246ae2e-39b3-4f3a-9205-67c4fcc95f18)

Run the following commands :

```bash
cd VSDBabySoC/src
sta
source sta.tcl
```

![Screenshot from 2024-11-04 17-48-55](https://github.com/user-attachments/assets/48baa28f-fe57-487b-b7d7-784e5db5fd1b)

![Screenshot from 2024-11-04 17-49-17](https://github.com/user-attachments/assets/da244e2a-784a-48d3-94d8-b24546b14517)

![Screenshot 2024-11-04 183132](https://github.com/user-attachments/assets/62450e51-99a0-4b9c-aa8a-a83f57679f75)


![Screenshot 2024-11-04 184427](https://github.com/user-attachments/assets/826eac97-8f63-4318-be2e-77432b21152f)


 Put the values in excel and plot the graphs :

 ![Screenshot 2024-11-04 182520](https://github.com/user-attachments/assets/22730cb5-8909-4ca1-9189-97b055060801)

![Screenshot 2024-11-04 182736](https://github.com/user-attachments/assets/68a92235-41dc-480d-8144-270b46e81720)


![Screenshot 2024-11-04 182751](https://github.com/user-attachments/assets/53f2d688-0aed-48cf-a71c-91de767dc221)


</details>


<details>
<summary>LAB 14: Advanced Physical Design using OpenLane using Sky130</summary>

# Day 1 : Inception of open-source EDA, OpenLane and Sky130 PDK

QFN-48 Package: The QFN-48 (Quad Flat No-leads) package is a compact, leadless integrated circuit package featuring 48 connection pads around its perimeter. Known for its excellent thermal and electrical performance, it is ideal for high-density applications.

![image](https://github.com/user-attachments/assets/9bf76c95-1d7b-41eb-a11c-f0bc9a44ece1)

A chip, or integrated circuit (IC), is a silicon-based component containing various functional blocks such as memory, processing units, and input/output interfaces. It is designed for specific applications in electronics.

![image](https://github.com/user-attachments/assets/6abedc89-2a60-4355-a8dd-a34fb4dc64b3)

Pads: Small metallic contact points on a chip or package that connect internal circuitry to external connections, enabling signal transfer to and from the IC.

Core: The central processing unit of a chip, containing functional logic and optimized for power and performance.

Die: The portion of a silicon wafer that contains an individual IC prior to packaging, housing all active circuitry required for the chip's functions.

![image](https://github.com/user-attachments/assets/a654879a-f3d5-4d38-bc62-e3a8ae6e9858)

IPs (Intellectual Properties): Pre-designed functional modules, like USB controllers or memory interfaces, that are licensed for reuse across multiple designs to reduce development time and costs.

![image](https://github.com/user-attachments/assets/96c6b1c8-f50a-4e8f-aa5a-21da2ced32f1)

#### Software to Hardware Execution Flow

To execute an application on hardware, it first passes through the system software layer, where it is translated into binary code that hardware can understand. This layer includes key components like the Operating System (OS), Compiler, and Assembler. The process begins with the OS, which decomposes functions written in high-level languages (such as C, C++, or Java) and sends them to an appropriate compiler. The compiler then converts these functions into low-level instructions tailored to the specific hardware architecture. Finally, the assembler translates these instructions into machine language, or binary code, which is fed to the hardware, allowing it to perform the tasks as defined by the application.


![image](https://github.com/user-attachments/assets/6f21d8ea-d86d-4375-bf29-7fbafda3cac3)

Consider a stopwatch app running on a RISC-V core. The OS generates a function in C, which is then passed to a compiler. The compiler outputs instructions specific to the RISC-V architecture. These instructions are processed by the assembler, which converts them into binary code. This binary code is subsequently integrated into the chip layout, enabling the hardware to execute the intended functionality of the stopwatch app.

![image](https://github.com/user-attachments/assets/e3befb78-617b-4768-8080-5921d5bc6716)

For the stopwatch example above, the figure below illustrates the input and output of both the compiler and the assembler.

![image](https://github.com/user-attachments/assets/ad7bbf77-ed8c-48a9-9347-becac87c0826)


The compiler generates architecture-specific instructions, which the assembler then converts into corresponding binary patterns. To execute these instructions on hardware, an RTL design, written in a Hardware Description Language, interprets and implements the instructions. This RTL design is synthesized into a netlist, represented by interconnected logic gates. The netlist then undergoes physical design implementation, preparing it for chip fabrication.

![image](https://github.com/user-attachments/assets/a07a70cc-7f4d-48b9-ae6c-2a0556193b78)

#### Components of ASIC Design


RTL IPs: Pre-designed, verified digital circuit blocks, such as adders, flip-flops, and memory, written in Hardware Description Languages (HDL) like Verilog or VHDL. These blocks save design time by providing ready-to-use components for building complex circuits.

EDA Tools: Software that automates key ASIC design tasks, including synthesis, optimization, placement, and timing analysis. EDA tools are essential for enhancing productivity and ensuring that performance and power requirements are met.

PDK Data: A collection of files and parameters provided by a semiconductor foundry that detail its manufacturing process, including transistor models and design rules. PDKs ensure that ASIC designs are compatible with the foundry's fabrication processes.


Simplified RTL to GDS flow

![image](https://github.com/user-attachments/assets/7b191df7-5857-4c5a-8c88-21bd301edd5a)


**RTL Design**: Defines circuit functionality using HDLs like Verilog or VHDL.

**RTL Synthesis**: Transforms RTL into a gate-level netlist with optimized standard cells.

**Floor and Power Planning**: Arranges major components, power grid, and I/O placement.

**Placement**: Assigns cell locations to minimize wirelength and delay.

**Clock Tree Synthesis (CTS)**: Ensures uniform clock signal distribution to reduce skew.

**Routing**: Connects components, adhering to design rules and optimizing paths.

**Sign-off**: Final checks to verify functionality, performance, power, and reliability.

**GDSII Generation**: Produces the final layout file for chip fabrication.



#### OpenLane ASIC Flow:

![image](https://github.com/user-attachments/assets/322c1ef3-9f55-415d-b0f1-50b8d539899c)

**RTL Synthesis and Technology Mapping**: Tools used are Yosys (for RTL synthesis) and ABC (for technology mapping and formal verification).  
**Static Timing Analysis**: OpenSTA is used for static timing analysis.  
**Floor Planning**: Tools include init_fp (initial floor planning), ioPlacer (I/O placement), pdn (power distribution network), and tapcell (tap cell insertion).  
**Placement**: Managed by RePLace (global placement), Resizer (for resizing cells), OpenPhySyn (placement), and OpenDP (detailed placement).  
**Clock Tree Synthesis**: TritonCTS is used for clock tree synthesis.  
**Fill Insertion**: OpenDP is used for filler placement.  
**Routing**: FastRoute or CU-GR for global routing, and TritonRoute or DR-CU for detailed routing.  
**SPEF Extraction**: OpenRCX is used for parasitic data extraction in SPEF format.  
**GDSII Output**: Magic and KLayout are used for viewing and editing GDSII files.  
**Design Rule Checks (DRC)**: Magic and KLayout are used for DRC checks.  
**Layout vs. Schematic (LVS) Check**: Netgen is used for LVS checks.  
**Antenna Checks**: Magic is used for antenna checks.


##### OpenLANE Directory structure

```bash
├── OOpenLane             -> directory where the tool can be invoked (run docker first)
│   ├── designs          -> All designs must be extracted from this folder
│   │   │   ├── picorv32a -> Design used as case study for this workshop
│   |   |   ├── ...
|   |   ├── ...
├── pdks                 -> contains pdk related files 
│   ├── skywater-pdk     -> all Skywater 130nm PDKs
│   ├── open-pdks        -> contains scripts that makes the commerical PDK (which is normally just compatible to commercial tools) to also be compatible with the open-source EDA tool
│   ├── sky130A          -> pdk variant made especially compatible for open-source tools
│   │   │  ├── libs.ref  -> files specific to node process (timing lib, cell lef, tech lef) for example is `sky130_fd_sc_hd` (Sky130nm Foundry Standard Cell High Density)  
│   │   │  ├── libs.tech -> files specific for the tool (klayout,netgen,magic...)
```


##### Synthesis in Openlane:

### Running Synthesis in OpenLane

Synthesis in openlane

```bash
cd Desktop/work/tools/openlane_working_dir/openlane
docker
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
run_synthesis
```

![image](https://github.com/user-attachments/assets/eede3466-1719-4045-9e56-d47732c12e0a)


![Screenshot 2024-11-12 173043](https://github.com/user-attachments/assets/5cb8ddca-f425-4e38-9012-5723f5079239)

View the netlist :

```bash
cd designs/picorv32a/runs/12-11_12-04/results/synthesis$
gedit picorv32a.synthesis.v
```

![image](https://github.com/user-attachments/assets/f2c9ce45-3ece-4965-9a33-f71149684ae4)

![image](https://github.com/user-attachments/assets/a81a8ff8-8089-4d89-91b2-52282931416c)

yosys report:

```bash
cd ..
cd ..
cd reports/synthesis
gedit 1-yosys_4.stat.rpt
```

![image](https://github.com/user-attachments/assets/39e0ae7b-c6fe-4e57-a151-26d9d0cc1b87)


![image](https://github.com/user-attachments/assets/a40de087-c244-4e36-9f5f-79acc6b02664)

![image](https://github.com/user-attachments/assets/0a24305e-4003-4116-b163-8345d4fd48bf)

```bash
28. Printing statistics.

=== picorv32a ===

   Number of wires:              14596
   Number of wire bits:          14978
   Number of public wires:        1565
   Number of public wire bits:    1947
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:              14876
     sky130_fd_sc_hd__a2111o_2       1
     sky130_fd_sc_hd__a211o_2       35
     sky130_fd_sc_hd__a211oi_2      60
     sky130_fd_sc_hd__a21bo_2      149
     sky130_fd_sc_hd__a21boi_2       8
     sky130_fd_sc_hd__a21o_2        57
     sky130_fd_sc_hd__a21oi_2      244
     sky130_fd_sc_hd__a221o_2       86
     sky130_fd_sc_hd__a22o_2      1013
     sky130_fd_sc_hd__a2bb2o_2    1748
     sky130_fd_sc_hd__a2bb2oi_2     81
     sky130_fd_sc_hd__a311o_2        2
     sky130_fd_sc_hd__a31o_2        49
     sky130_fd_sc_hd__a31oi_2        7
     sky130_fd_sc_hd__a32o_2        46
     sky130_fd_sc_hd__a41o_2         1
     sky130_fd_sc_hd__and2_2       157
     sky130_fd_sc_hd__and3_2        58
     sky130_fd_sc_hd__and4_2       345
     sky130_fd_sc_hd__and4b_2        1
     sky130_fd_sc_hd__buf_1       1656
     sky130_fd_sc_hd__buf_2          8
     sky130_fd_sc_hd__conb_1        42
     sky130_fd_sc_hd__dfxtp_2     1613
     sky130_fd_sc_hd__inv_2       1615
     sky130_fd_sc_hd__mux2_1      1224
     sky130_fd_sc_hd__mux2_2         2
     sky130_fd_sc_hd__mux4_1       221
     sky130_fd_sc_hd__nand2_2       78
     sky130_fd_sc_hd__nor2_2       524
     sky130_fd_sc_hd__nor2b_2        1
     sky130_fd_sc_hd__nor3_2        42
     sky130_fd_sc_hd__nor4_2         1
     sky130_fd_sc_hd__o2111a_2       2
     sky130_fd_sc_hd__o211a_2       69
     sky130_fd_sc_hd__o211ai_2       6
     sky130_fd_sc_hd__o21a_2        54
     sky130_fd_sc_hd__o21ai_2      141
     sky130_fd_sc_hd__o21ba_2      209
     sky130_fd_sc_hd__o21bai_2       1
     sky130_fd_sc_hd__o221a_2      204
     sky130_fd_sc_hd__o221ai_2       7
     sky130_fd_sc_hd__o22a_2      1312
     sky130_fd_sc_hd__o22ai_2       59
     sky130_fd_sc_hd__o2bb2a_2     119
     sky130_fd_sc_hd__o2bb2ai_2     92
     sky130_fd_sc_hd__o311a_2        8
     sky130_fd_sc_hd__o31a_2        19
     sky130_fd_sc_hd__o31ai_2        1
     sky130_fd_sc_hd__o32a_2       109
     sky130_fd_sc_hd__o41a_2         2
     sky130_fd_sc_hd__or2_2       1088
     sky130_fd_sc_hd__or2b_2        25
     sky130_fd_sc_hd__or3_2         68
     sky130_fd_sc_hd__or3b_2         5
     sky130_fd_sc_hd__or4_2         93
     sky130_fd_sc_hd__or4b_2         6
     sky130_fd_sc_hd__or4bb_2        2

   Chip area for module '\picorv32a': 147712.918400
```

```bash
Flop ratio = Number of D Flip flops = 1613  = 0.1084
             ______________________   _____
             Total Number of cells    14876
```

Wire Count: 14,596
Cell Count: 14,876, including specific cells like sky130_fd_sc_hd__a2111o_2, sky130_fd_sc_hd__and2_2.
D Flip-flops: 1,613 with a flop ratio of 0.1084


# Day 2 : Good floorplan vs bad floorplan and introduction to library cells

Utilization Factor and Aspect Ratio: In IC floor planning, the utilization factor and aspect ratio are critical parameters. The utilization factor is the ratio of the netlist area to the total core area, with an ideal value of 1 (100%). However, practical designs typically aim for a factor between 0.5 and 0.6 to allow space for buffer zones, routing channels, and future adjustments. The aspect ratio, calculated as height divided by width, defines the chip’s shape. An aspect ratio of 1 results in a square layout, while other values produce a rectangular layout. The specific aspect ratio is chosen based on functional, packaging, and manufacturing requirements.

```bash
Utilisation Factor =  Area occupied by netlist
                     __________________________
                         Total area of core
                         

Aspect Ratio =  Height
               ________
                Width
```

**Pre-placed Cells**: Key functional blocks like memory, custom processors, and analog circuits that are manually positioned in fixed locations. These cells are crucial for performance and remain fixed during placement and routing to maintain layout integrity.

**Decoupling Capacitors**: Capacitors placed near logic circuits to stabilize power supply voltages during transients. They act as local energy reserves to reduce voltage fluctuations, crosstalk, and EMI, ensuring reliable power for sensitive circuits.

**Power Planning**: Involves creating a power and ground mesh to distribute VDD and VSS evenly across the chip, ensuring stable power delivery, reducing voltage drops, and improving efficiency. Multiple power and ground points minimize instability risks.

**Pin Placement**: Strategic placement of I/O pins to enhance functionality and reliability. Proper pin positioning minimizes signal degradation, aids in heat dissipation, and supports thermal stability, while power and ground pin placement enhances signal strength and manufacturability.

#### Floorplaning using Openlane :

Run the commands :

```bash
cd Desktop/work/tools/openlane_working_dir/openlane
docker
```
```bash
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
run_synthesis
run_floorplan
```

![image](https://github.com/user-attachments/assets/cffe0696-1f36-48fd-8bc2-04f5b3f9e046)

![image](https://github.com/user-attachments/assets/aa0fc19d-6023-4f7c-9e15-8407a6942788)

![image](https://github.com/user-attachments/assets/635e7937-7d6a-4a40-b80d-2a1cde0890a6)



Run the following commands in a new terminal:

```bash
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/12-11_16-16/results/floorplan
gedit picorv32a.floorplan.def
```
![image](https://github.com/user-attachments/assets/84387a61-4a22-4a6f-b37b-11e3b78b101f)

![image](https://github.com/user-attachments/assets/143724c9-ee51-443f-88c5-97bc465660c7)

![image](https://github.com/user-attachments/assets/da5c6ac7-afe0-4932-8ae1-3aae2dab19a9)


According to the floorplan definitions:

1000 Unit Distance = 1 Micron

Die width in unit distance = 660685−0 = 660685

Die height in unit distance = 671405−0 = 671405

Distance in microns = Value in Unit Distance/1000

​Die width in microns = 660685/1000 = 660.685 Microns

Die height in microns = 671405/1000 = 671.405 Microns

Area of die in microns = 660.685 × 671.405 = 443587.212425 Square Microns

view the floorplan in magic by opening a new terminal and runbing the below commands:


```bash
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/12-11_16-16/results/floorplan
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```

![image](https://github.com/user-attachments/assets/110d8fe0-c142-4e74-b8a4-354267d99aa1)

![image](https://github.com/user-attachments/assets/1ec3080c-46c7-4a31-8e98-9acfec45a86c)

![image](https://github.com/user-attachments/assets/f07aecd1-9fd0-4103-9097-968627856263)


Run placement

```bash
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
run_synthesis
run_floorplan
run_placement
```

![image](https://github.com/user-attachments/assets/fd26e19d-c05d-45aa-827b-7fe6cd02d9af)

view placemnet in magic :

```bash
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/12-11_16-16/results/placement
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

![image](https://github.com/user-attachments/assets/76b36f0e-cb65-4b9f-ae87-d67564d2204a)

![image](https://github.com/user-attachments/assets/2fe60812-de7b-420a-a26b-6bf1e3b29b32)

**Cell Design and Characterization Flow**  
The library provides information on each cell, including various sizes, functionalities, and threshold voltages. A typical cell design flow involves:

- **Inputs**: Process Design Kits (PDKs) for DRC & LVS, SPICE models, and library/user-defined specifications.
- **Design Steps**: Circuit design, layout design (using Euler’s path and stick diagrams), parasitic extraction, and characterization for timing, noise, and power.
- **Outputs**: CDL, LEF, GDSII, extracted SPICE netlist (.cir), and timing, noise, and power .lib files.

**Standard Cell Characterization Flow**  
A standard cell characterization flow in the industry includes the following steps:

1. Read models and technology files.
2. Load the extracted SPICE netlist.
3. Recognize cell behavior.
4. Read subcircuits.
5. Apply power sources.
6. Stimulate the characterization setup.
7. Set output capacitance loads.
8. Run simulation commands.

These steps are combined into a configuration file for the GUNA software, which generates timing, noise, and power models, producing .lib files for timing, power, and noise characterization.

Timing parameters

| **Timing Definition**   | **Value** |
|-------------------------|-----------|
| slew\_low\_rise\_thr    | 20%       |
| slew\_high\_rise\_thr   | 80%       |
| slew\_low\_fall\_thr    | 20%       |
| slew\_high\_fall\_thr   | 80%       |
| in\_rise\_thr           | 50%       |
| in\_fall\_thr           | 50%       |
| out\_rise\_thr          | 50%       |
| out\_fall\_thr          | 50%       |

Propagation Delay: Time for an input change to reach 50% of its final value and affect the output similarly.

```bash
rise delay =  time(out_fall_thr) - time(in_rise_thr)
```

Transistion time: The time it takes for the signal to move between states is the transition time , where the time is measured between 10% and 90% or 20% to 80% of the signal levels.

```bash
Fall transition time: time(slew_high_fall_thr) - time(slew_low_fall_thr)
Rise transition time: time(slew_high_rise_thr) - time(slew_low_rise_thr)
```

# Day 3 : Design library cell using Magic Layout and ngspice characterization

#### CMOS inverter ngspice simulations

Creating a SPICE Deck for CMOS Inverter Simulation

1. **Netlist Creation**: Define the netlist by specifying the component connections in the CMOS inverter circuit. Label each node (e.g., input, output, ground, and supply nodes) for easy identification in SPICE.

2. **Device Sizing**: Set the Width-to-Length (W/L) ratios for the PMOS and NMOS transistors. To balance drive strength, the PMOS width should typically be 2x to 3x larger than the NMOS width.

3. **Voltage Levels**: Configure the gate and supply voltages, often in multiples of the transistor length, based on design requirements.

4. **Node Naming**: Assign names to each connection point (e.g., VDD, GND, IN, OUT) to help SPICE recognize and simulate each component effectively.


![image](https://github.com/user-attachments/assets/b4f48cf4-760e-4d66-b131-ca352a15fbe3)

```bash
***syntax for PMOS and NMOS desription***
[component name] [drain] [gate] [source] [substrate] [transistor type] W=[width] L=[length]

 ***simulation commands***
.op --- is the start of SPICE simulation operation where Vin sweeps from 0 to 2.5 with 0.5 steps
tsmc_025um_model.mod  ----  model file which contains the technological parameters for the 0.25um NMOS and PMOS
```

To simulate in SPICE: 

```bash
source [filename].cir
run
setplot 
dc1 
plot out vs in
```

![image](https://github.com/user-attachments/assets/b951a968-4fd7-4ff7-b7f4-6c08269d8201)

To find the switching threshold :

The switching threshold Vm is the critical voltage level for a CMOS inverter. At this point, the inverter switches between outputting a "0" or a "1" in a chip. Here, both PMOS and NMOS transistors are in the saturation region, meaning both are partially turned on, leading to a high chance of leakage current (current flowing directly from VDD to ground). 

When the PMOS transistor is thicker than the NMOS, the CMOS inverter will have a higher switching threshold (e.g., 1.2V instead of 1V). Conversely, the threshold will be lower when the NMOS is thicker.


```bash
Vin in 0 2.5
.op
.dc Vin 0 2.5 0.05
```

![image](https://github.com/user-attachments/assets/080cf3ec-7b18-4b3a-8f78-73e3c0f5927b)

Simulation commands :

```bash
Vin in 0 0 pulse 0 2.5 0 10p 10p 1n 2n 
.op
.tran 10p 4n
```

SPICE simulation for transient analysis :

![image](https://github.com/user-attachments/assets/55d6568b-a03b-4b7e-a87c-1e939e16446b)

Now, clone the custom inverter :

```bash
cd Desktop/work/tools/openlane_working_dir/openlane
git clone https://github.com/nickson-jose/vsdstdcelldesign
cd vsdstdcelldesign
cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .
ls
magic -T sky130A.tech sky130_inv.mag &
```

![image](https://github.com/user-attachments/assets/0598b0e8-7cea-4b23-a944-e7d5c96c9316)

![image](https://github.com/user-attachments/assets/9b80082d-9257-4afe-86f0-67d0b0e7c265)

####  Inception of Layout CMOS fabrication process

The 16-mask CMOS design fabrication process:

- **Substrate Preparation**: Starts with preparing a silicon wafer as the foundational substrate for the circuit.
- **N-Well Formation**: N-well regions are created by introducing phosphorus or other impurities through ion implantation or diffusion.
- **P-Well Formation**: P-well regions are similarly created using boron or other dopants through ion implantation or diffusion.
- **Gate Oxide Deposition**: A thin silicon dioxide layer is deposited to form the gate oxide, insulating the gate from the channel.
- **Poly-Silicon Deposition**: A polysilicon layer is deposited on the gate oxide to serve as the gate electrode.
- **Poly-Silicon Masking and Etching**: A photoresist mask defines areas where polysilicon should remain, followed by etching to remove exposed portions.
- **N-Well Masking and Implantation**: A photoresist mask defines preserved N-well areas, where phosphorus is implanted into exposed regions.
- **P-Well Masking and Implantation**: A photoresist mask defines preserved P-well areas, where boron is implanted into exposed regions.
- **Source/Drain Implantation**: Using photoresist masks, dopants are implanted to form source and drain regions (arsenic for NMOS, boron for PMOS).
- **Gate Formation**: The gate electrode is defined by etching the polysilicon layer with a photoresist mask.
- **Source/Drain Masking and Etching**: A photoresist mask is applied to define the source and drain regions, followed by oxide layer etching.
- **Contact/Via Formation**: Contact holes or vias are etched through the oxide layer to expose regions like source/drain or polysilicon gates.
- **Metal Deposition**: A metal layer (typically aluminum or copper) is deposited to create interconnects.
- **Metal Masking and Etching**: A photoresist mask defines metal interconnects, followed by etching to remove the unwanted metal, leaving desired patterns.
- **Passivation Layer Deposition**: A protective layer, such as silicon dioxide or nitride, is deposited to isolate and shield the metal interconnects.
- **Final Testing and Packaging**: The fabricated wafer undergoes testing to ensure functionality, then working chips are separated, packaged, and prepared for use.


![image](https://github.com/user-attachments/assets/2d2332ce-6d0e-4024-8ee4-c76e95604bbd)

Inverter layout :

![Screenshot 2024-11-13 140412](https://github.com/user-attachments/assets/ed589900-7879-4ab5-8f86-64ff2f4875d5)

![Screenshot 2024-11-13 140549](https://github.com/user-attachments/assets/ad4abc94-b9d3-44bf-b130-4997301bd3b2)

![Screenshot 2024-11-13 140707](https://github.com/user-attachments/assets/110f3e5a-1e4b-44e2-9a73-402837aaf791)

![Screenshot 2024-11-13 140748](https://github.com/user-attachments/assets/8bd4074b-cb95-4e2b-8a34-f98dd577ac6f)

![Screenshot 2024-11-13 140820](https://github.com/user-attachments/assets/8c2077ff-0357-4469-9808-45dc644cd97b)

![Screenshot 2024-11-13 140820](https://github.com/user-attachments/assets/84e399a1-31c5-4a6f-a18e-16e89f34979b)
SPICE Extraction with Magic :

```bash
pwd 
extract all 
ext2spice cthresh 0 rthresh 0 
ext2spice
```
![Screenshot 2024-11-13 141201](https://github.com/user-attachments/assets/fe01cd44-1f7c-459c-8d30-767e77394a08)

Modifying SPICE File for Transient Analysis

![Screenshot 2024-11-13 141708](https://github.com/user-attachments/assets/cb1fb785-8807-4a85-99f1-755927ae299b)

Edit sky130_inv.spice:

```bash
* SPICE3 file created from sky130_inv.ext - technology: sky130A

.option scale=0.01u
.include ./libs/pshort.lib
.include ./libs/nshort.lib

M1000 Y A VGND VGND nshort_model.0 w=35 l=23
+  ad=1.44n pd=0.152m as=1.37n ps=0.148m
M1001 Y A VPWR VPWR pshort_model.0 w=37 l=23
+  ad=1.44n pd=0.152m as=1.52n ps=0.156m

VDD VPWR 0 3.3V
VSS VGND 0 0V
Va A VGND PULSE(0V 3.3V 0 0.1ns 0.1ns 2ns 4ns)

.tran 1n 20n
.control
run
.endc
.end
```

simulate :

```bash
ngspice sky130_inv.spice 
plot y vs time a
```
![Screenshot 2024-11-13 142112](https://github.com/user-attachments/assets/cbd4273c-6332-4798-8e58-f4d97837eb70)

![Screenshot 2024-11-13 142150](https://github.com/user-attachments/assets/35902de2-4571-4e4a-bb72-b907f19c34c9)


Characterizing Slew Rate and Propagation Delay Using Transient Response:

- **Rise Transition**: Time taken for the output to rise from 20% to 80% of the maximum value.
- **Fall Transition**: Time taken for the output to fall from 80% to 20% of the maximum value.
- **Cell Rise Delay**: Difference in time between the 50% output rise and the 50% input fall.
- **Cell Fall Delay**: Difference in time between the 50% output fall and the 50% input rise.


Example calculation :

```bash
Rise Transition : 2.24638 - 2.18242 =  0.06396 ns = 63.96 ps
Fall Transition : 4.0955 - 4.05536 =  0.0419 ns = 41.9 ps
Cell Rise Delay : 2.21144 - 2.15008 = 0.06136 ns = 61.36 ps
Cell Fall Delay : 4.07807 - 4.05 =0.02 ns = 20 ps
```

#### Magic Tool DRC Rules Check

Set up and run:

```bash
cd 
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz 
tar xfz drc_tests.tgz 
cd drc_tests 
gvim .magicrc 
magic -d XR &
```
![Screenshot 2024-11-13 142911](https://github.com/user-attachments/assets/7ba7a772-4e3b-4230-9135-863e9cf2e584)

![Screenshot 2024-11-13 143130](https://github.com/user-attachments/assets/d5a8d814-15da-4de4-9a3a-be62b82fe49c)

DRC commands :

```bash
tech load sky130A.tech 
drc check 
drc why
```
![Screenshot 2024-11-13 144321](https://github.com/user-attachments/assets/0bb92fdd-1f0e-4875-8893-a0282f29064c)

![Screenshot 2024-11-13 144624](https://github.com/user-attachments/assets/7fd9e839-6516-4ce2-99a0-940d95ba26c0)


# Day 4 : Pre-layout timing analysis and importance of good clock tree

tracks.info file:

```bash
cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign
cd ../../pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd/
less tracks.info
```

![Screenshot 2024-11-13 184043](https://github.com/user-attachments/assets/c1eb81a6-8e4a-49e2-a5d5-d2b9662c809e)

```bash
grid 0.46um 0.34um 0.23um 0.17um
```

![Screenshot 2024-11-13 191746](https://github.com/user-attachments/assets/e06d2853-c622-4ba9-9aa2-e77b7d47d331)

Give a custom name :

```bash
save sky130_yasinv.mag
```

![Screenshot 2024-11-13 191845](https://github.com/user-attachments/assets/dd9a8ebd-536d-42b9-82de-6cdb5d6d26df)

![Screenshot 2024-11-13 192224](https://github.com/user-attachments/assets/933b31ed-6ab6-42c3-a5d0-cffa7fb4c3b8)

Open it by using the following commands:

```bash
magic -T sky130A.tech sky130_yasinv.mag &
```

type the following command in tkcon window:

```bash
lef write
```

![Screenshot 2024-11-13 192532](https://github.com/user-attachments/assets/c034c6d5-168a-4d16-b269-f6e036e37551)

![Screenshot 2024-11-13 194831](https://github.com/user-attachments/assets/a8d18e11-9dc8-4a07-b981-e64a4e9faf9c)

Modify config.tcl:

```bash
# Design
set ::env(DESIGN_NAME) "picorv32a"

set ::env(VERILOG_FILES) "./designs/picorv32a/src/picorv32a.v"
set ::env(SDC_FILES) "./designs/picorv32a/src/picorv32a.sdc"


set ::env(CLOCK_PERIOD) "5.000"
set ::env(CLOCK_PORT) "clk"

set ::env(CLOCK_NET) $::env(CLOCK_PORT) 


set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib "
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib "
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"

set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]   ## this is the new line added to the existing config.tcl file

set filename $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/$::env(PDK)_$::env(STD_CELL_LIBRARY)_config.tcl
if { [file exists $filename] == 1 } {
  source $filename
}
```
Run the openlane flow synthesis :

```bash
cd Desktop/work/tools/openlane_working_dir/openlane
docker
```
```bash
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
run_synthesis
```
![Screenshot 2024-11-13 194843](https://github.com/user-attachments/assets/414edd2c-e174-4271-ae60-edba199409b0)

![Screenshot 2024-11-13 195133](https://github.com/user-attachments/assets/9a6000b3-e3c1-4ad4-935a-57eb0016fbb8)

![Screenshot 2024-11-13 195147](https://github.com/user-attachments/assets/9b5b1af4-6107-40d5-8103-19ed51e2660b)

Delay Tables

Delay plays a crucial role in cell timing, impacted by input transition and output load. Cells of the same type can have different delays depending on wire length due to resistance and capacitance variations. To manage this, "delay tables" are created, using 2D arrays with input slew and load capacitance for each buffer size as timing models. Algorithms compute buffer delays from these tables, interpolating where exact data isn’t available to estimate delays accurately, preserving signal integrity across varying load conditions.

![image](https://github.com/user-attachments/assets/744ff28f-6a40-4931-973b-c41dcf0ce77f)

Fixinf Slack :

```bash
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a -tag 13-11_14-18 -overwrite
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
echo $::env(SYNTH_STRATEGY)
set ::env(SYNTH_STRATEGY) "DELAY 3"
echo $::env(SYNTH_BUFFERING)
echo $::env(SYNTH_SIZING)
set ::env(SYNTH_SIZING) 1
echo $::env(SYNTH_DRIVING_CELL)
run_synthesis
```

![image](https://github.com/user-attachments/assets/e4a94e7c-22b8-4203-affa-a4f8e5ae85ed)

![image](https://github.com/user-attachments/assets/7de93fe5-558a-4b2c-9304-35d46f47672c)

![image](https://github.com/user-attachments/assets/197df5e0-24ae-445f-87ef-2c32d4f5b496)


```bash
run_floorplan
```
![image](https://github.com/user-attachments/assets/53acff90-e28d-46b6-9fa0-a725a48be321)

Since we are encountering an unexpected, unexplainable error with the `run_floorplan` command, we can instead use the following set of commands available based on information from `Desktop/work/tools/openlane_working_dir/openlane/scripts/tcl_commands/floorplan.tcl` and the *Floorplan Commands* section in `Desktop/work/tools/openlane_working_dir/openlane/docs/source/OpenLANE_commands.md`.


```bash
init_floorplan
place_io
tap_decap_or
```

```bash
run_placement
```

![image](https://github.com/user-attachments/assets/fd9ebbfd-1a08-4add-ad24-0e564fd4c486)

In a new terminal run the below commands to load placement def in magic

```bash
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_14-18/results/placement/
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

![image](https://github.com/user-attachments/assets/27762dcc-b17e-4ff9-a5fa-cd4ebfaba522)


Custom inverter inserted in placement def

![Screenshot 2024-11-13 213232](https://github.com/user-attachments/assets/a2f12e7d-ac14-41ed-b1b9-37244229dfba)

![Screenshot 2024-11-13 213131](https://github.com/user-attachments/assets/086c503f-c320-47b1-8bc0-82a08bc24555)

![Screenshot 2024-11-13 213216](https://github.com/user-attachments/assets/4ed59475-f2aa-46d8-876f-da540f8d2668)

![Screenshot 2024-11-13 213142](https://github.com/user-attachments/assets/faf14675-7890-49f7-857f-f2e43a876ab5)


![image](https://github.com/user-attachments/assets/fe3533b1-aec3-415c-a1ed-f5ac2bc2d073)



#### Timing analysis with ideal clocks using openSTA

Pre-layout STA will include effects of clock buffers and net-delay due to RC parasitics (wire delay will be derived from PDK library wire model).

![image](https://github.com/user-attachments/assets/c8db25dd-c4cf-418c-83b9-ef723aa8970e)

we are getting 0 wns after improved timing run, we will be doing the timing analysis on initial run of synthesis which has lots of violations and no parameters added to improve timing.

```bash
cd Desktop/work/tools/openlane_working_dir/openlane
docker
./flow.tcl -interactive
package require openlane 0.9set
prep -design picorv32a
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
set ::env(SYNTH_SIZING) 1
run_synthesis
```

Go, to Desktop/work/tools/openlane_working_dir/openlane and create a file pre_sta.conf. The contents of the file are:

```bash
set_cmd_units -time ns -capacitance pF -current mA -voltage V -resistance kOhm -distance um
read_liberty -max /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib
read_liberty -min /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib
read_verilog /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_14-18/results/synthesis/picorv32a.synthesis.v
link_design picorv32a
read_sdc /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/my_base.sdc
report_checks -path_delay min_max -fields {slew trans net cap input_pin}
report_tns
report_wns
```
Commands to run STA:

```bash
cd Desktop/work/tools/openlane_working_dir/openlane
sta pre_sta.conf
```

<img width="959" alt="image" src="https://github.com/user-attachments/assets/7e4b90be-7612-43c8-a400-836bcb8b3115">

Go to new terminal and run the following commands:

```bash
cd Desktop/work/tools/openlane_working_dir/openlane
docker
./flow.tcl -interactive
prep -design picorv32a -tag 13-11_14-18 -overwrite
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
set ::env(SYNTH_SIZING) 1
set ::env(SYNTH_MAX_FANOUT) 4
echo $::env(SYNTH_DRIVING_CELL)
run_synthesis
```

to run sta

```bash
cd Desktop/work/tools/openlane_working_dir/openlane
sta pre_sta.conf
```

Run the following commands to optimise timing:

```bash
report_net -connections _13111_
replace_cell _16171_ sky130_fd_sc_hd__nor3_2
report_checks -fields {net cap slew input_pins} -digits 4
```
![image](https://github.com/user-attachments/assets/5fafd314-f88e-4873-bd11-7ba213a438aa)


Clock Tree Synthesis (CTS) using TritonCTS involves different techniques tailored to meet specific design requirements:

- **Balanced Tree CTS**: This method builds a balanced, binary-like tree structure to ensure equal path lengths from the clock source to each clock sink, effectively minimizing clock skew. While providing good timing accuracy, it typically offers moderate power efficiency.

- **H-tree CTS**: This technique utilizes an "H"-shaped layout, which is particularly effective for covering large chip areas. It supports better power efficiency and is commonly used in designs where uniform clock distribution across a wide area is essential.


![image](https://github.com/user-attachments/assets/b0630499-929e-4c57-8220-289d38a1ebed)


Clock Tree Synthesis (CTS) techniques used in TritonCTS also include:

- **Star CTS**: Distributes the clock signal from a central point, minimizing skew across the network. However, it typically requires additional buffers near the source to drive the clock signal effectively.

- **Global-Local CTS**: Combines both star and tree topologies, using a global tree for distributing clocks across different domains and local trees within each domain. This approach balances global and local timing requirements, making it suitable for complex designs.

- **Mesh CTS**: Utilizes a grid-like pattern for clock distribution, ideal for highly structured designs. This approach balances simplicity with low skew, offering robustness in designs with uniform clock distribution needs.

- **Adaptive CTS**: Adjusts clock tree paths dynamically based on timing and congestion considerations, providing flexibility in design. While it offers advantages in complex designs, it introduces added complexity in implementation.

Crosstalk

Crosstalk is interference from overlapping electromagnetic fields between adjacent circuits, causing unwanted signals. In VLSI, it can lead to data corruption, timing issues, and higher power consumption. Mitigation strategies include optimized layout and routing, shielding, and clock gating to reduce dynamic power and minimize crosstalk effects

<img width="572" alt="image" src="https://github.com/user-attachments/assets/e4538821-0fe0-4971-9e7a-b2bed1bea372">


Clock Net Shielding

Clock net shielding prevents glitches by isolating the clock network, using shields connected to VDD or GND that don’t switch. It reduces interference by isolating clocks from other signals, often with dedicated routing layers and clock buffers. Additionally, clock domain isolation helps prevent cross-domain interference, avoiding metastability and maintaining synchronization.

<img width="600" alt="image" src="https://github.com/user-attachments/assets/fd58cb17-4ec7-49c6-a9aa-0a051a81d3e0">

to insert this updated netlist to PnR flow and we can use write_verilog and overwrite the synthesis netlist but before that we are going to make a copy of the old old netlist:

```bash
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_14-18/results/synthesis/
ls
cp picorv32a.synthesis.v picorv32a.synthesis_old.v
ls
```
Run the following commands:

```bash
write_verilog /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_14-18/results/synthesis/picorv32a.synthesis.v
exit
```

<img width="959" alt="image" src="https://github.com/user-attachments/assets/ac13b7d2-84b6-4809-a4ed-146d6c927337">

```bash
cd Desktop/work/tools/openlane_working_dir/openlane
docker
./flow.tcl -interactive
prep -design picorv32a -tag 13-11_14-18 -overwrite
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
set ::env(SYNTH_STRATEGY) "DELAY 3"
set ::env(SYNTH_SIZING) 1
run_synthesis
init_floorplan
place_io
tap_decap_or
run_placement
run_cts
```

![Screenshot 2024-11-13 233924](https://github.com/user-attachments/assets/80a410d4-b5a8-46df-bbf9-e18e602ee3b9)

![Screenshot 2024-11-13 234313](https://github.com/user-attachments/assets/0ddc6da0-bdcf-43dd-bfdd-5bc8d1a10998)

Setup timing analysis using real clocks

A real clock in timing analysis accounts for practical factors like clock skew and clock jitter. Clock skew is the difference in arrival times of the clock signal at different parts of the circuit due to physical delays, which affects setup and hold timing margins. Clock jitter is the variability in the clock period caused by power, temperature, and noise fluctuations, leading to uncertainty in clock edge timing. Both factors are crucial for accurate timing analysis, ensuring the design performs reliably in real-world conditions.

<img width="588" alt="image" src="https://github.com/user-attachments/assets/77a5606d-9faa-4c90-baeb-2bf594652bd1">

enter the following commands for Post-CTS OpenROAD timing analysis:

```bash
openroad
read_lef /openLANE_flow/designs/picorv32a/runs/13-11_14-18/tmp/merged.lef
read_def /openLANE_flow/designs/picorv32a/runs/13-11_14-18/results/cts/picorv32a.cts.def
write_db pico_cts.db
read_db pico_cts.db
read_verilog /openLANE_flow/designs/picorv32a/runs/13-11_14-18/results/synthesis/picorv32a.synthesis_cts.v
read_liberty $::env(LIB_SYNTH_COMPLETE)
link_design picorv32a
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc
set_propagated_clock [all_clocks]
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4
exit
```

<img width="959" alt="image" src="https://github.com/user-attachments/assets/4ff50f64-44fd-4de8-865f-a8ebec85c7cd">

```bash
echo $::env(CTS_CLK_BUFFER_LIST)
set ::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0]
echo $::env(CTS_CLK_BUFFER_LIST)
echo $::env(CURRENT_DEF)
set ::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/13-11_14-18/results/placement/picorv32a.placement.def
run_cts
echo $::env(CTS_CLK_BUFFER_LIST)
openroad
read_lef /openLANE_flow/designs/picorv32a/runs/13-11_14-18/tmp/merged.lef
read_def /openLANE_flow/designs/picorv32a/runs/13-11_14-18/results/cts/picorv32a.cts.def
write_db pico_cts1.db
read_db pico_cts.db
read_verilog /openLANE_flow/designs/picorv32a/runs/13-11_14-18/results/synthesis/picorv32a.synthesis_cts.v
read_liberty $::env(LIB_SYNTH_COMPLETE)
link_design picorv32a
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc
set_propagated_clock [all_clocks]
report_checks -path_delay min_max -fields {slew transd net cap input_pins} -format full_clock_expanded -digits 4
report_clock_skew -hold
report_clock_skew -setup
exit
echo $::env(CTS_CLK_BUFFER_LIST)
set ::env(CTS_CLK_BUFFER_LIST) [linsert $::env(CTS_CLK_BUFFER_LIST) 0 sky130_fd_sc_hd__clkbuf_1]
echo $::env(CTS_CLK_BUFFER_LIST)
```

<img width="959" alt="image" src="https://github.com/user-attachments/assets/413df5cc-35de-4c07-bab2-ab8bab2ceb25">

<img width="958" alt="image" src="https://github.com/user-attachments/assets/d16e49bd-72b2-4ba3-bc12-482c1f30e266">

# Day 5 : Day 5 - Final steps for RTL2GDS using tritonRoute and openSTA

#### Perform generation of Power Distribution Network (PDN) and explore the PDN layout.

Ru the Commands to perform all necessary stages

```bash

cd Desktop/work/tools/openlane_working_dir/openlane
docker

./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
set ::env(SYNTH_STRATEGY) "DELAY 3"
set ::env(SYNTH_SIZING) 1
run_synthesis
init_floorplan
place_io
tap_decap_or
run_placement
run_cts
gen_pdn
```

<img width="959" alt="image" src="https://github.com/user-attachments/assets/464ecb38-0e74-44f6-9d99-9b0724ebd225">

<img width="959" alt="image" src="https://github.com/user-attachments/assets/da79338a-022d-4cab-ad42-1bfbf036600b">

<img width="959" alt="image" src="https://github.com/user-attachments/assets/d3eb77de-b03e-44d4-935e-c3240905530c">

<img width="959" alt="image" src="https://github.com/user-attachments/assets/9a26752c-c9ca-4a1c-bcfd-9fd481401c94">

![Screenshot 2024-11-13 213232](https://github.com/user-attachments/assets/627fc5be-624b-45c0-8c13-d57a9eab6cc1)

![image](https://github.com/user-attachments/assets/fac2e0af-9213-4adb-a58d-67cec07ba8f4)


#### Perfrom detailed routing using TritonRoute and explore the routed layout

```bash
echo $::env(CURRENT_DEF)
echo $::env(ROUTING_STRATEGY)
run_routing
```

<img width="959" alt="image" src="https://github.com/user-attachments/assets/59ad8c87-d2fd-4973-809c-4fb503d0d09c">

![Screenshot 2024-11-13 213232](https://github.com/user-attachments/assets/627fc5be-624b-45c0-8c13-d57a9eab6cc1)

screenshot of fast route guide 

<img width="959" alt="image" src="https://github.com/user-attachments/assets/89445d79-f520-4712-9596-5e2b017b19ee">

#### Post-Route parasitic extraction using SPEF extractor.

```bash

cd Desktop/work/tools/SPEF_EXTRACTOR

python3 main.py /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_14-18/tmp/merged.lef /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_14-18/results/routing/picorv32a.def
```

#### Post-Route OpenSTA timing analysis with the extracted parasitics of the route.

Commands to be run in OpenLANE flow to do OpenROAD timing analysis with integrated OpenSTA in OpenROAD

```bash
openroad
read_lef /openLANE_flow/designs/picorv32a/runs/13-11_14-18/tmp/merged.lef
read_def /openLANE_flow/designs/picorv32a/runs/13-11_14-18/results/routing/picorv32a.def
write_db pico_route.db
read_db pico_route.db
read_verilog /openLANE_flow/designs/picorv32a/runs/13-11_14-18/results/synthesis/picorv32a.synthesis_preroute.v
read_liberty $::env(LIB_SYNTH_COMPLETE)
link_design picorv32a
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc
set_propagated_clock [all_clocks]
read_spef /openLANE_flow/designs/picorv32a/runs/13-11_14-18/results/routing/picorv32a.spef
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4
exit
```

</details>

<details>
<summary>LAB 15: OpenRoad Physical Design</summary>

#### Path to Zetta-Scale Computing

Introduction :

**Bombe:** The Bombe, developed during WWII by Alan Turing and Gordon Welchman at Bletchley Park, was an electro-mechanical machine designed to decrypt German Enigma-encrypted messages. By systematically testing rotor settings based on known plaintext patterns, it dramatically reduced the time needed to find the correct keys, playing a crucial role in the Allied war effort.  

**ENIAC:** Developed during WWII by John Presper Eckert and John Mauchly, ENIAC was the first fully electronic general-purpose digital computer. Completed in 1945, it used vacuum tubes and was primarily designed to compute artillery firing tables for the U.S. Army. Though powerful, it required manual reconfiguration for new tasks, as it lacked a stored-program capability.  

**EDVAC:** EDVAC, completed in 1949, improved on ENIAC by introducing the stored-program concept, binary representation, and the ability to store both data and instructions in memory. Designed by Eckert, Mauchly, and John von Neumann, it marked a major step toward modern computing with its influence on the von Neumann architecture.

50 Years of Microprocessor Trend Data:

![Screenshot from 2024-11-26 00-28-22](https://github.com/user-attachments/assets/9f1e3cfc-8c42-4297-9fad-0309128d2d03)

**Key Metrics:**  

- **Transistors (Orange Triangles):** The number of transistors on microprocessor chips has grown exponentially, doubling approximately every two years as predicted by Moore's Law. This enabled increasingly complex processors, reaching billions of transistors by the 2020s.  
- **Single-Thread Performance (Blue Circles):** Measured using SpecINT, it reflects the computational ability of a single processor core. While performance steadily improved through advancements in architecture, instruction-level parallelism, and clock speeds, growth slowed post-2005 due to physical limits like power and heat.  
- **Frequency (Green Diamonds):** Processor clock speeds increased steadily until the early 2000s but plateaued due to inefficiencies and heat dissipation challenges.  
- **Typical Power (Red Triangles):** Power consumption rose alongside transistor density and frequency, becoming a significant design constraint by the mid-2000s.  
- **Number of Logical Cores (Black Dots):** Multi-core processors became prevalent in the mid-2000s as a solution to stagnant single-thread performance. Increasing core counts enabled efficient parallel processing and boosted overall performance.  

**Key Milestones:**  
- **iPhone Release (~2007):** Marked the rise of mobile computing, emphasizing power efficiency alongside performance, spurring advancements in low-power processor designs.  
- **Datacenter-Scale Computing (Post-2010):** Highlighted the importance of energy efficiency, scalability, and parallelism in cloud computing and large-scale data centers.

Path to zetta-scale computing

![Screenshot from 2024-11-26 00-28-41](https://github.com/user-attachments/assets/1d9f098b-ab72-41ab-af4c-6c8b07a5a171)

**The Path to Zettascale Computing:**  

Tracing the growth of computing performance from the gigascale era in 1984 to the anticipated zettascale by 2035, measured in FLOPS (floating-point operations per second).  

**Key Performance Levels:**  
- **Gigascale (10⁹ FLOPS):** The starting point in 1984, marking the capabilities of early supercomputers.  
- **Terascale (10¹² FLOPS):** Achieved around 1997 with systems like Jaguar (Cray XT5), delivering teraflop performance at 7 MW power consumption.  
- **Petascale (10¹⁵ FLOPS):** Reached in 2008 with systems like Titan (Cray XK6), offering 27 petaflops while consuming 9 MW of power. This era marked significant advancements in high-performance computing (HPC).  
- **Exascale (10¹⁸ FLOPS):** Attained by 2021 with systems like Frontier (Cray Shasta), achieving 1.5 exaflops using 4 AMD GPUs and 1 AMD CPU, consuming 29 MW. Exascale systems enable large-scale AI workloads and highly detailed simulations.  
- **Zettascale (10²¹ FLOPS):** Expected by 2035, these systems will tackle massive computational tasks, including advanced climate modeling and AI. Power consumption is estimated at 50–100 MW for a single zettascale machine.

CMOS Evolution and Next-Gen Candidates

![Screenshot from 2024-11-26 00-29-01](https://github.com/user-attachments/assets/edcf966e-1d9d-4160-acbe-829bf8f3fde5)

**Evolving CMOS Technology Landscape:**  

This diagram highlights advancements in materials, structures, and processes aimed at overcoming the challenges of scaling CMOS technology to the 1nm node and beyond.  

### **Key Areas of Innovation:**  

**1. Channel Material:**  
- **Current Trends:** Silicon (Si) dominates as the channel material, with strained SiGe (Silicon-Germanium) used for enhanced carrier mobility in high-performance applications.  
- **Future Materials:** 2D materials like MoS₂ and Germanium (Ge) are being explored for their superior electrical properties at smaller scales.  

**2. Patterning:**  
- **Current Techniques:** Deep Ultraviolet (DUV) lithography, using ArF and KrF lasers, is the standard for defining transistor features.  
- **Next-Gen:** Extreme Ultraviolet (EUV) lithography and High-NA EUV are crucial for achieving sub-7nm nodes with better resolution.  

**3. Gate Stack Material:**  
- **Current Materials:** High-K metal gates (HKMG) minimize leakage and improve switching performance in modern FETs.  
- **Next-Gen Candidates:**  
  - **NC-FET (Negative Capacitance FET):** Uses ferroelectric materials to lower power consumption.  
  - **TFET (Tunnel FET):** Leverages quantum tunneling for efficient switching, ideal for low-power applications.  

**4. Interconnection Material:**  
- **Current Materials:** Copper (Cu) is widely used for its low resistivity, ensuring minimal power loss.  
- **Next-Gen Materials:** Ruthenium (Ru), compound metals, and topological semi-metals are being explored for reduced resistance and improved performance at smaller scales.  

**5. Device Structure:**  
- **Current Architectures:** FinFET and planar transistors offer effective performance control, with FinFETs addressing short-channel effects using 3D designs.  
- **Next-Gen Architectures:**  
  - **3DS-FET (3D Stacked FET):** Vertical stacking for improved performance and reduced footprint.  
  - **MBC-FET (Multi-Bridge Channel FET):** Multiple channels in one device to boost drive current.  
  - **VFET (Vertical FET):** Uses vertical channels for better density and power efficiency.  

**6. Design Co-Optimization:**  
- **DTCO (Design-Technology Co-Optimization):** Integrates advanced design techniques, such as backside interconnects (BSI), to improve signal integrity and reduce latency.  
- **STCO (System-Technology Co-Optimization):** Optimizes system architecture and technology with innovations like chiplets, which enable modular, flexible designs for easier scaling and reduced complexity.  

FinFETs :

![Screenshot from 2024-11-26 00-29-20](https://github.com/user-attachments/assets/f837e77e-23b6-46fd-87e0-0c225a86e06b)

**Evolution of Transistor Technology:**  

This diagram outlines the progression from planar transistors to advanced architectures like FinFET and Gate-All-Around (GAA), addressing the challenges of miniaturization and performance.  

### **Key Milestones:**  

**1. Planar Transistor (Traditional):**  
- Features a flat channel with the gate controlling the channel from one side only.  
- Performance limitations arise as scaling continues, leading to leakage and reduced efficiency.  

**2. FinFET (2011):**  
- The channel is shaped like a vertical fin, enabling the gate to wrap around three sides of the channel.  
- Provides enhanced control over the channel, reducing leakage and improving performance at smaller sizes.  

**3. Gate-All-Around (GAA) Transistor (2025?):**  
- The gate completely surrounds the channel, implemented with stacked nanosheets or nanowires.  
- Offers superior channel control compared to FinFET, supporting higher performance and efficiency as scaling progresses.  

### **Impact:**  
Each advancement improves drive current capability, enhances control over on/off states, and supports greater power efficiency and miniaturization in modern electronics.

Why FinFETs and Gate-All-Around Transistors?

![Screenshot from 2024-11-26 00-29-41](https://github.com/user-attachments/assets/5721b832-c4a4-4c5a-a96a-88d006d5f629)

**Advantages of FinFETs and Gate-All-Around (GAA) Transistors Over Planar Structures:**  

This diagram highlights the key benefits of transitioning from planar transistors to FinFETs and GAA transistors to address efficiency and scaling challenges.  

### **Planar Transistors:**  
- **Challenges:**  
  - Sub-channel leakage due to limited gate control, reducing efficiency.  
  - Higher power consumption from leakage currents.  

### **FinFET Transistors:**  
- **Structure:** The gate wraps around three sides of the channel (fin), enhancing control.  
- **Benefits:**  
  - Reduces sub-threshold leakage, improving power efficiency.  
  - Enhances drive current (\(I_{ON}\)) for better performance.  
  - Allows for a smaller transistor footprint while maintaining high performance.  

### **Gate-All-Around (GAA) Transistors:**  
- **Structure:** The gate completely surrounds the channel, offering superior electrostatic control.  
- **Advantages:**  
  - Improves short-channel performance by reducing drain capacitance and enhancing gate capacitance.  
  - Better scaling efficiency, as described by \( S \propto (1 + C_d / C_{ox}) \), with lower \(C_d\) and higher \(C_{ox}\).  
  - Reduces sub-threshold slope, achieving superior performance at smaller scales.  

### **Graph Comparison:**  
- Demonstrates the improved efficiency and reduced sub-threshold slope of FinFETs and GAA transistors compared to planar transistors.  
- Highlights the significant performance advantages of these advanced architectures as dimensions shrink.  

![Screenshot from 2024-11-26 00-30-01](https://github.com/user-attachments/assets/6e869c45-373b-47bf-920e-b0070962f98a)

**Advantages of Tri-Gate Transistors and FEOL Innovations:**  

### **Tri-Gate Transistors:**  
- **Reduced Leakage:**  
  - Tri-Gate transistors exhibit significantly lower leakage currents compared to planar transistors at the same gate voltage.  
  - This leads to reduced off-current, lower power dissipation, and improved energy efficiency.  
- **Higher Drive Current:**  
  - Provide higher drive current at the same off-current level compared to planar designs.  
  - Results in better circuit performance and greater efficiency, ideal for modern electronic applications.  

### **FEOL (Front-End-Of-Line) Innovations:**  
- **Definition:**  
  - FEOL refers to the early stages of semiconductor manufacturing, focusing on building active devices like transistors, capacitors, and isolation structures on the silicon wafer before adding metal interconnects.  
- **Impact:**  
  - FEOL innovations enable the development of smaller, more efficient, and powerful transistors.  
  - These advancements are critical to sustaining Moore's Law and driving progress in semiconductor technology.  

CMOS Technology Inflection Points

![Screenshot from 2024-11-26 00-30-16](https://github.com/user-attachments/assets/b5a028a6-4fdf-4970-9596-6b7fcc0180d4)

**Dennard Scaling and Technology Node Innovations:**  

### **Dennard Scaling:**  
- **Concept:** As transistors shrink, power density remains constant, enabling voltage scaling with smaller gate lengths.  
- **Drive Voltage Scaling Graph (Bottom-left):**  
  - Illustrates the relationship between gate length (logarithmic scale) and drive voltage (logarithmic scale).  
  - Ideal scaling shows linear voltage reduction with shrinking gate length.  
  - Practical trends (red and green markers) for low-power and high-performance devices deviate from ideal scaling due to challenges like leakage currents and increased power density.  

### **Technology Nodes and Key Innovations:**  
1. **~1 µm ("End of Scaling"):** Marks the onset of CMOS miniaturization.  
2. **180 nm (Voltage Scaling):** Drive voltage reduction begins.  
3. **130 nm (Cu BEOL):** Introduction of copper interconnects for improved conductivity.  
4. **90 nm (Uniaxial Strained Si NMOS):** Strained silicon enhances electron mobility for better performance.  
5. **65 nm (eSiGe CVD ULK):** Embedded SiGe enhances PMOS performance by applying strain.  
6. **45 nm (HK-first MG-last):** Use of high-k dielectrics and metal gates reduces leakage and improves gate control.  
7. **32 nm (HKMG with Raised S/D NMOS):** Advanced high-k metal gate (HKMG) and raised source/drain regions boost performance.  

### **SEM Images:**  
- **Left Image:**  
  - Shows a cross-section of a transistor with high-k materials and embedded SiGe.  
  - High-k dielectrics and metal gates improve performance, while SiGe enhances PMOS efficiency by applying strain to the silicon channel.  
- **Right Image:**  
  - Depicts raised source/drain (S/D) regions and the gate channel in PMOS transistors, demonstrating innovations at smaller nodes.

![Screenshot from 2024-11-26 00-30-43](https://github.com/user-attachments/assets/4d016023-45de-471c-aee4-76823e0d8849)

**Key Technology Nodes and Innovations:**  

### **22 nm:**  
- **FinFET (Tri-Gate) Transistors:**  
  - Reduced leakage and improved gate control with 3D architecture.  
- **Self-Aligned Contacts (SAC) and Copper Interconnects (Co+Cu BEOL):**  
  - Enhanced performance and reduced resistivity.  

### **14 nm:**  
- **Unidirectional Metal Routing:**  
  - Improved density and signal integrity.  
- **Advanced Layout Techniques:**  
  - SADP (Self-Aligned Double Patterning) for precise feature definition.  
  - SDB (Single Diffusion Break) for better transistor isolation.  

### **10 nm:**  
- **Advanced Patterning Techniques:**  
  - SA-SDB (Self-Aligned SDB), LELELE (Litho-Etch-Litho-Etch-Litho-Etch), and SAQP (Self-Aligned Quadruple Patterning) for tighter geometries.  

### **7 nm:**  
- **Extreme Ultraviolet Lithography (EUV):**  
  - Simplifies patterning and reduces overlay errors.  

### **5 nm:**  
- **SiGe Channels for PMOS:**  
  - Enhanced hole mobility for better performance.  
- **EUV SA-LELE:**  
  - Improved precision in lithography processes.  

### **3 nm / 2 nm / 1.4 nm:**  
- **Gate-All-Around (GAA) Transistors:**  
  - Stacked nanosheets or nanowires horizontally for improved electrostatic control and higher current drive.  

### **Sub-1 nm:**  
- **CFET (Complementary FET):**  
  - Vertically stacks NMOS over PMOS to save area.  
- **2D Materials (e.g., MoS₂):**  
  - Enables atomic-scale channel thickness for next-generation 2D FETs.  

![Screenshot from 2024-11-26 00-31-06](https://github.com/user-attachments/assets/ba10d065-834c-4ea5-91eb-b5972f6b32e3)

**Samsung's Transistor Scaling with Fin Depopulation:**  

The image highlights Samsung's approach to scaling transistors across their technology nodes (10nm, 8nm, 7nm, and 5nm) using **Fin Depopulation**. In FinFET transistors, the "fin" acts as a vertical channel carrying the current. By reducing the number of fins per transistor while keeping the fin width constant, Samsung achieves smaller transistors without sacrificing performance.  

### **Technology Nodes and Fin Depopulation Details:**  
- **10nm (HD):**  
  - **Fin Height:** 420nm  
  - **Number of Fins:** 10  

- **8nm (UHD):**  
  - **Fin Height:** 378nm  
  - **Number of Fins:** 9  

- **7nm (HD):**  
  - **Fin Height:** 27nm  
  - **Number of Fins:** 8  

- **5nm (UHD):**  
  - **Fin Height:** 27nm  
  - **Number of Fins:** 7  

This systematic reduction in fins supports smaller transistor dimensions while maintaining efficiency and performance, enabling Samsung to advance their process technology and chip capabilities.  

![Screenshot from 2024-11-26 00-31-34](https://github.com/user-attachments/assets/37f26a0b-f284-466a-b7a1-1fb64c2df715)

**Key Techniques for Transistor Scaling and Density Improvement:**

### **Double Diffusion Break (DDB):**  
- **Description:**  
  - DDB involves creating a gap between the source and drain regions of a transistor, filled with an insulating material. This reduces the effective width of the transistor.  
  - **Benefit:**  
    - Enables smaller cell sizes, which allows for higher transistor density and improved scalability.
  - **Illustration:**  
    - A cross-section of the transistor shows the insulating gap between the source and drain regions.

### **Single Diffusion Break (SDB):**  
- **Description:**  
  - Similar to DDB but less aggressive, SDB introduces a gap only on one side of the transistor.
  - **Benefit:**  
    - Provides a balance between size reduction and maintaining transistor performance.
  - **Illustration:**  
    - A cross-section highlights the gap on one side of the transistor, showcasing its simplicity compared to DDB.

### **Contact Over Field Gate (COFG):**  
- **Description:**  
  - COFG places the gate contact directly over the field oxide region of a transistor.
  - **Benefit:**  
    - Reduces lateral spacing between adjacent transistors, enabling smaller cell sizes without significant performance loss.
  - **Illustration:**  
    - A cross-section shows the gate contact positioned over the field oxide region.

### **Contact Over Active Gate (COAG):**  
- **Description:**  
  - COAG is a more aggressive technique where the gate contact is placed directly over the active gate region of the transistor.
  - **Benefit:**  
    - Enables even smaller cell sizes and higher transistor density, which is critical for advanced semiconductor nodes.
  - **Illustration:**  
    - A cross-sectional image highlights the gate contact placement over the active gate region.

### **Back-Side Power Delivery Network (BS-PDN):**  
- **Description:**  
  - BS-PDN routes power supply rails on the backside of the chip.
  - **Benefit:**  
    - Reduces the height of the standard cell, creating space for more transistors, and improves power delivery efficiency and resistance.
  - **Illustration:**  
    - A schematic of the standard cell shows the positioning of power rails on the backside of the chip, enhancing transistor density.

![Screenshot from 2024-11-26 00-31-48](https://github.com/user-attachments/assets/fcb12a5f-c893-4e04-9986-1eea1ec142b3)

**Key Technology Advances in Threshold Voltage (Vt) Variability:**

### **Planar Technology:**  
- **Description:**  
  - In early planar technology nodes (100nm and above), threshold voltage (Vt) variability is high, around **130mV**.
- **Cause:**  
  - Variability arises from factors such as process variations, temperature fluctuations, and line-edge roughness.
  
### **FinFET Technology:**  
- **Description:**  
  - With the advent of FinFET technology (around **22nm**), Vt variability significantly reduces to around **14mV**.  
- **Improvement:**  
  - This reduction is due to better control over the channel length and width in FinFETs compared to planar transistors, leading to more stable performance.

### **NW (Nanowire) Technology:**  
- **Description:**  
  - In the latest **nanowire** technology (14nm and below), Vt variability drops even further to around **7mV**.  
- **Improvement:**  
  - The reduction in variability is due to precise control over nanowire dimensions and the minimized impact of process variations, offering superior stability and performance at smaller nodes.
 
![Screenshot from 2024-11-26 00-32-26](https://github.com/user-attachments/assets/02ee4115-4d35-4355-b75a-5f26e53e7469)

### **Key Differences in Parasitic Resistance Across Transistor Architectures:**

---

### **Planar MOSFETs:**  
- **Structure:**  
  - Traditional architecture with the gate sitting above the channel.  
- **Contact to Gate Width Ratio:**  
  - **\( W_C / W_G \approx 1 \)**, meaning the contact width is nearly equal to the gate width.  
- **Parasitic Resistance:**  
  - **\( R_{EXT} \ll R_{ch} \)**, with external resistance (parasitic resistance) much smaller than channel resistance.  
  - **Impact on Performance:**  
    - Minimal performance degradation due to parasitic resistance.

---

### **FinFETs:**  
- **Structure:**  
  - 3D design with vertical fins and the gate wrapping around the fins for better control.  
- **Contact to Gate Width Ratio:**  
  - **\( W_C / W_G \approx 1/3 \)**, meaning the contact width is smaller relative to the gate width.  
- **Parasitic Resistance:**  
  - **\( R_{EXT} / R_{ch} \approx 1 \)**, making parasitic resistance comparable to the channel resistance.  
  - **Impact on Performance:**  
    - Parasitic resistance starts to affect performance as FinFETs scale.

---

### **Gate-All-Around (GAA) FETs:**  
- **Structure:**  
  - Uses nanosheets or nanowires, with the gate fully surrounding the channel for superior electrostatic control.  
- **Contact to Gate Width Ratio:**  
  - **\( W_C / W_G \approx 1/6 \)**, meaning the contact width is much smaller than the gate width.  
- **Parasitic Resistance:**  
  - **\( R_{EXT} / R_{ch} \approx 3 \)**, indicating significant parasitic resistance compared to the channel resistance.  
  - **Impact on Performance:**  
    - While GAA FETs offer improved transistor density, the higher parasitic resistance becomes a major challenge in maintaining high performance.

---

### **Complementary FETs (CFETs):**  
- **Structure:**  
  - Integrates both NMOS and PMOS transistors vertically to improve space efficiency.  
- **Contact to Gate Width Ratio:**  
  - Similar to GAA FETs, **\( W_C / W_G \)** remains small.  
- **Parasitic Resistance:**  
  - **\( R_{EXT} / R_{ch} \approx 3 \)**, inheriting the high parasitic resistance of GAA FETs.  
  - **Impact on Performance:**  
    - Similar challenges as GAA FETs in balancing transistor density with parasitic resistance, affecting performance.

Explanation of Parasitic Resistance

![Screenshot from 2024-11-26 00-32-40](https://github.com/user-attachments/assets/8ab3c9de-5ae4-49cb-bd9b-fe981d8b0d16)

### **Parasitic Resistance Breakdown and Reduction in Transistors:**

---

### **Components of Parasitic Resistance \((R_{EXT})\):**  
The diagram highlights the key contributors to parasitic resistance in a transistor:

- **\( R_{CA-BEOL} \)**: Resistance from the contact in the Back-End-Of-Line (BEOL) process.
- **\( R_{CA} \)**: Resistance at the contact area itself.
- **\( R_{CA-TS} \)**: Resistance between the contact and the transition structure.
- **\( R_{TS} \)**: Resistance within the transition structure.
- **\( R_{MOL} \)**: Middle-Of-Line resistance, which includes both lateral and vertical contributions.
- **\( R_C \)**: Contact resistance at the metal-semiconductor interface.
- **\( R_{EPI} \)**: Epitaxial layer resistance, contributing to current spreading.
- **\( R_{FEOL} \)**: Front-End-Of-Line resistance, particularly from source/drain extensions.

---

### **Initial vs. Improved \( R_{EXT} \) Breakdown:**

**NFETs (N-Channel FETs):**
- **Initial:**  
  - **\( R_C \)**: 63%  
  - **\( R_{CA-BEOL} \)**: 31%
- **Improved:**  
  - **\( R_C \)** reduced to 48%  
  - **\( R_{CA-BEOL} \)** reduced to 12%

**PFETs (P-Channel FETs):**
- **Initial:**  
  - **\( R_{FEOL} \)**: 50%  
  - **\( R_C \)**: 45%
- **Improved:**  
  - **\( R_{FEOL} \)** reduced to 78%  
  - **\( R_C \)** reduced to 16%

---

### **Improving Ohmic/Tunneling Contacts:**

- **Key Objectives for Improving Contact Resistance:**
  - **Low Schottky Barrier Height \((\phi_b)\):** Reduces the energy barrier for carrier injection, enhancing contact conductivity.
  - **High Doping Concentration \((N_d)\):** Increases carrier density at the metal-semiconductor interface, reducing contact resistance.

- **Equation for Specific Contact Resistivity \((\rho_c)\):**  
  \[
  \rho_c \propto \exp\left(\frac{2\phi_b}{\hbar} \sqrt{\frac{\epsilon_s m_x}{N_d}}\right)
  \]
  - The equation shows that by lowering \(\phi_b\) and increasing \(N_d\), the contact resistivity \(\rho_c\) can be reduced.

---

### **Metal-Semiconductor Energy Diagram:**
- The energy diagram illustrates how a reduction in the Schottky Barrier Height \((\phi_b)\) facilitates easier carrier injection from the metal to the semiconductor, improving overall contact performance.

![Screenshot from 2024-11-26 00-32-57](https://github.com/user-attachments/assets/732ccac3-a2f3-4e58-96d0-434835699560)

### **Capacitance Composition Evolution Across Nodes (22nm to 7nm):**

---

### **Bar Chart Breakdown (Left Side):**

- **22nm Technology Node:**
  - **\( C_{fr} \)** (Fringe Capacitance): Dominates at **56%**.
  - **\( C_{pc-ca} \)** (Parasitic Capacitance - Contact-Acting): **25%**.
  - **\( C_g \)** (Gate Capacitance): **19%**.

- **14nm and 10nm Technology Nodes:**
  - **\( C_{fr} \)** decreases significantly: 38% at 14nm and 25% at 10nm.
  - **\( C_{pc-ca} \)** increases as a dominant factor in the capacitance composition.

- **7nm Technology Node:**
  - **\( C_g \)** becomes the largest contributor to **45%**.
  - **\( C_{pc-ca} \)** and **\( C_{fr} \)** decrease further, indicating a shift towards a more significant impact of gate capacitance and parasitic effects as technology nodes shrink.

---

### **Plot Descriptions:**

- **First Scatter Plot:**
  - Shows a **reduction in normalized delay** for a **ring oscillator** when using **SiBCN spacers** instead of **SiN spacers**, demonstrating **improved performance** with the new spacer material.

- **Second Scatter Plot:**
  - Demonstrates an **8% reduction in \( C_{eff} \)** with **SiBCN spacers**, aligning with the performance improvement noted in the first plot, emphasizing the benefit of using SiBCN in reducing effective capacitance.

- **Spacer Materials Evolution (Rightmost Figure):**
  - Shows the transition of **spacer materials** from **SiOCN** to **SiCO**, highlighting an effort to **reduce gate-contact capacitance** across technology nodes. 
  - At 14nm and beyond, **low-\(k\)** spacers are employed to **decouple parasitic effects** and maintain **manageable capacitance levels**.

- **Bottom Middle Image (TEM View of Transistor with Air Spacers):**
  - Displays a **cross-sectional TEM view** of a transistor with **air spacers** around the gate, showing that air’s **low dielectric constant (\(k \approx 1\))** significantly reduces **parasitic capacitance**.
  - The adjacent plot quantifies this improvement, showing a **15% reduction in \( C_{eff} \)** when using air spacers compared to solid spacers, further emphasizing the advantage of air as a spacer material.

---

This diagram highlights how parasitic capacitance components evolve with shrinking technology nodes and the strategies (such as using SiBCN spacers and air spacers) employed to manage and reduce capacitance, improving transistor performance.

![Screenshot from 2024-11-26 00-33-13](https://github.com/user-attachments/assets/eb65720e-bda0-49d7-806b-8ced292ca9e6)

### **Key Properties of 2D Layered Materials (Compared to Silicon):**

---

1. **Uniform Atomic Scale Thickness:**
   - A single layer of **MoS₂** is approximately **0.65 nm** thick, providing an ideal thickness for scaling compared to silicon, which is typically much thicker at the atomic scale.
   - This ultra-thin structure allows for more compact device designs and is beneficial for **scalability** in modern electronics.

2. **Higher Effective Mass (\( m^* \)):**
   - **MoS₂** has an effective mass of about **0.55 times** the mass of a free electron (**\( m_0 \)**), while silicon’s effective mass is **around 0.22 \( m_0 \)**.
   - This higher effective mass in MoS₂ can impact the **carrier mobility** and **electronic transport properties**, potentially leading to differences in how the material behaves in devices compared to silicon.

3. **Bandgap:**
   - 2D materials like **MoS₂** possess **favorable bandgaps** for electronic devices. A **monolayer** of MoS₂ has a **bandgap of approximately 1.85 eV**, while a **bilayer** reduces the bandgap to about **1.5 eV**.
   - This bandgap range is ideal for **electronic switching** and **optical applications**, offering **better control over current flow** and enhancing **performance in transistors** when compared to silicon's smaller bandgap.

---

This diagram highlights the key advantages of 2D layered materials like MoS₂, emphasizing their potential to outperform traditional silicon in terms of thickness, effective mass, and bandgap, which can enable **more efficient and scalable electronic devices**.

![Screenshot from 2024-11-26 00-33-42](https://github.com/user-attachments/assets/5cda675c-2a80-487f-9940-f173ba4a8e16)

### **Transistor Scaling:**

---

#### **Challenges for Scaling:**

1. **Direct Source-to-Drain Tunneling:**
   - As gate lengths decrease, **electrons** can tunnel directly from the source to the drain, bypassing the gate's control over the current flow.
   - This phenomenon leads to **increased leakage** and can cause devices to fail to turn off properly.
   - **Mitigation Strategy:** Using materials with a **high effective mass (\(m^*\))**, which helps reduce tunneling by improving the gate's control over the channel.

2. **Surface Roughness and Thickness Variations:**
   - At atomic scales, **surface roughness** and **thickness variations** can lead to **performance degradation** due to **variability** in the channel material properties.
   - **Solution:** **Uniform, atomically thin materials** like **2D materials** are essential to reduce these variations and ensure consistent performance across devices.

3. **Capacitance Ratios (\(C_D\) and \(C_{ox}\)):**
   - The **capacitance of the depletion region (\(C_D\))** must remain low relative to the **gate oxide capacitance (\(C_{ox}\))** for improved gate control and switching efficiency.
   - **Challenge:** As devices shrink, ensuring the proper capacitance ratio is increasingly difficult.
   - **Required Materials:** Materials with a **low in-plane dielectric constant (\(\epsilon\))** are needed to maintain these ratios and ensure proper transistor operation.

---

#### **Diagrams:**

1. **Left Diagram:**
   - A cross-sectional view of the transistor structure shows key components:
     - **Source**
     - **Drain**
     - **Gate**
     - **Oxide**
     - **Silicon Substrate**
   
2. **Right Diagram:**
   - **Scenario a - Thermionic Emission:**
     - Electrons cross the energy barrier from the source to the drain as intended, ensuring normal transistor operation.
   
   - **Scenario b - Direct Tunneling:**
     - At extremely small gate lengths, electrons can tunnel directly from the source to the drain, leading to leakage and degraded performance.

![Screenshot from 2024-11-26 00-33-57](https://github.com/user-attachments/assets/ceb10129-3d08-4631-9b2a-3dc740d883ea)

### **Concept of Direct Source-to-Drain Tunneling:**

As the gate length (\(L_G\)) of MOSFETs decreases, **direct tunneling** of electrons from the **source** to the **drain** becomes increasingly significant. This phenomenon leads to **increased leakage currents**, which can severely affect the performance and energy efficiency of the device.

#### **Key Factors Influencing Tunneling Leakage:**

1. **Effective Mass (\(m^*\)):**
   - A **higher effective mass** in the channel material makes it more difficult for electrons to tunnel through the channel, thereby **reducing leakage** currents.
   - Materials like **MoS₂** have a higher \(m^*\) compared to silicon, making them more effective in suppressing tunneling leakage.

2. **Bandgap (\(E_G\)):**
   - The **bandgap** of the material plays a crucial role in determining the likelihood of tunneling.
   - A **larger bandgap** (like MoS₂'s ~1.85 eV for a monolayer) offers better resistance to tunneling compared to materials with smaller bandgaps like silicon, further minimizing leakage.

3. **Dielectric Constant (\(\epsilon\)):**
   - A **lower dielectric constant** in materials like MoS₂ reduces the overall **capacitance** and helps in further reducing leakage currents.

---

#### **Graph Description:**

- The graph plots **leakage current (\(I_{SD, \text{leak}}\))** as a function of **gate length (\(L_G\))** for various **channel thicknesses (\(T_{CH}\))**.
- **MoS₂** shows significantly lower leakage at **smaller gate lengths** compared to **silicon**, achieving up to **100x reduction in leakage**.
- This is due to the combined effects of **higher effective mass (\(m^*\))**, **larger bandgap (\(E_G\))**, and **lower dielectric constant (\(\epsilon\))** in MoS₂.

![Screenshot from 2024-11-26 00-34-15](https://github.com/user-attachments/assets/d6263836-bd33-4842-bf17-0d25be290bcb)

### **MoS₂ Transistor with 1 nm Gate Length:**

The **MoS₂ transistor** with a **1 nm gate length** represents a **breakthrough in transistor miniaturization**, featuring a **thin MoS₂ channel** that leverages the excellent **electronic properties** of the material.

#### **Key Components and Materials:**

1. **MoS₂ Channel:**
   - The **MoS₂ channel** provides **outstanding electronic properties** due to its **2D nature**, which allows for **strong gate control** at very small scales, essential for scaling down transistors.
  
2. **Single-Walled Carbon Nanotube (SWCNT) Gate Electrode:**
   - A **SWCNT gate** serves as the **ultra-small gate electrode**, offering **excellent electrical conductivity** and **high surface area**, crucial for efficient modulation of the MoS₂ channel at the nanoscale.

3. **Zirconium Dioxide (ZrO₂) High-k Dielectric:**
   - **ZrO₂** is used as the **high-k dielectric**, which improves **gate control** and **reduces leakage currents** compared to traditional dielectrics, enabling better performance at smaller scales.

4. **SiO₂ Substrate and n⁺ Silicon Back Gate:**
   - The transistor is **built on a SiO₂ substrate** with an **n⁺ silicon back gate**, which provides **additional control over the device** and enhances the **modulation** of the MoS₂ channel.

#### **Design and Operation:**

- The **SWCNT gate** **depletes a small region** of the MoS₂ channel, allowing for **precise modulation** of the channel conductance, which is essential for controlling the **on-off switching** of the transistor.
  
- This innovative design demonstrates the **potential of 2D materials** like MoS₂ and **nanoscale gates** in **advancing transistor technology**, offering **greater scalability** and **performance** in future **nanoelectronics**.

![Screenshot from 2024-11-26 00-34-38](https://github.com/user-attachments/assets/1be78787-dcce-40e4-ac5a-a7175860a4eb)

The slide illustrates the structure and fabrication of an **All-2D MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor)**, where all key components, including the channel, gate, and contacts, are made using two-dimensional materials. This device leverages the exceptional properties of 2D materials to improve performance and scalability. Below is a breakdown of the key components and the fabrication process:

- **Graphene Contacts (S, D, G):** Graphene is used as the source, drain, and gate electrodes. Its high conductivity and 2D nature make it ideal for ensuring low-resistance electrical contacts.

- **MoS₂ Channel:** Molybdenum Disulfide (MoS₂) serves as the semiconductor channel. MoS₂ is widely used in 2D MOSFETs due to its excellent on/off current ratio and atomic-scale thickness.

- **h-BN Dielectric:** Hexagonal Boron Nitride (h-BN) acts as the insulating dielectric layer. It is a 2D material with excellent insulating properties and high thermal stability, making it suitable for separating the graphene gate from the MoS₂ channel.

- **Si/SiO₂ Substrate:** The device is fabricated on a silicon dioxide (SiO₂) layer on top of a silicon substrate, which provides mechanical support and a global back gate.

**Fabrication Process:**

1. A layer of graphene is deposited on the SiO₂ substrate, which will later serve as the gate electrode.
2. Graphene is patterned to define the source and drain regions, leaving gaps for the channel.
3. A monolayer of MoS₂ is transferred onto the patterned graphene, forming the channel region.
4. An h-BN layer is added on top of the MoS₂ as the gate dielectric.
5. A top layer of graphene is deposited and aligned as the gate electrode.
6. The completed device is contacted using metallic electrodes (Ni/Au) for testing.


![Screenshot from 2024-11-26 00-35-07](https://github.com/user-attachments/assets/c27e206b-387c-4292-b179-fcb53b9ca8b4)


The **All-2D MOSFET** exhibits excellent electrical performance:

- **Transfer Characteristics (I(_D) vs V(_G))**: Achieves a high on/off current ratio (>10⁵), demonstrating strong gate control for effective switching.
  
- **Output Characteristics (I(D) vs V({DS}))**: Smooth current modulation with increasing V(G) and V({DS}), indicating good output performance.

- **Mobility**: Field-effect mobility remains constant with increasing gate electric field, showing minimal scattering and high-quality interfaces in the 2D materials stack. 

These results highlight the potential of 2D materials like MoS₂, graphene, and h-BN for scalable, high-performance transistor applications.

![Screenshot from 2024-11-26 00-35-23](https://github.com/user-attachments/assets/56135500-acec-4050-9243-b3c43c8aea8e)


The diagram on the top left shows a **non-planar transistor** with key components:

- **Gate**: Controls the flow of current through the channel.
- **Channel**: Region where current flows between the source (S) and drain (D).
- **Body**: Underlying region connected to the substrate.
- **STI (Shallow Trench Isolation)**: Insulates neighboring devices.

The biggest challenge is to form single-crystalline semiconductors on a non-planar surface, which is difficult using conventional semiconductor fabrication techniques.

![Screenshot from 2024-11-26 00-36-20](https://github.com/user-attachments/assets/0007dc16-5f63-4fbd-b9f0-1205c669c2ba)

- **Single-Layer CMOS (a)**: This is the traditional CMOS design where NMOS and PMOS transistors are fabricated on a single silicon layer. Each transistor operates in the same planar layer, with devices connected laterally.

- **Monolithic 3D CMOS (b)**: In this design, NMOS and PMOS transistors are stacked in two separate layers, enabling higher density. The P-Channel (PMOS) is placed on top of the N-Channel (NMOS), separated by an oxide layer. This approach reduces the footprint and allows better performance due to shorter interconnects.

- **Single-Layer CMOS Logic (c)**: Shows logic gates (inverter, 2-input NAND, and 2-input NOR) built using traditional single-layer CMOS. Transistors are laid out horizontally, with interconnections taking more space.

- **Monolithic 3D CMOS Logic (d)**: Logic gates are constructed with two transistor layers (Layer 1 and Layer 2), reducing the area required for interconnections. Vertical integration improves performance and reduces delay by shortening signal paths.


![Screenshot from 2024-11-26 00-36-37](https://github.com/user-attachments/assets/c600b0d5-5455-4464-805c-8cffe8ba8d33)

- **Dual Damascene Cu (7nm Technology Node)**: Utilized for a 36nm pitch, this technique combines vias (vertical connections) and lines (horizontal connections) in a single patterning step. Copper (Cu) serves as the interconnection material, but shrinking dimensions introduce challenges such as gap filling and maintaining reliability.

- **Single Damascene Cu (5nm Technology Node)**: Used for a 28nm pitch, this process separates the creation of vias and lines into distinct steps. It addresses the challenges of reduced dimensions by focusing on minimizing resistance (R) in both lines and vias to sustain optimal performance.

- **Barrier and Via Metal Optimization (3nm Technology Node)**: Implemented for a 20-24nm pitch, this approach reduces the thickness of barrier layers to lower resistance while ensuring robust and reliable via connections. It is critical to meet the scaling and performance requirements of advanced nodes.

- **Subtractive RIE and Alternative Metals (Sub-18nm Pitch)**: This stage introduces subtractive Reactive Ion Etching (RIE) for precise interconnect patterning and alternative metals like ruthenium (Ru) to improve reliability and performance. These advancements address the challenges of scaling traditional copper interconnects.

- **Post-Damascene Interconnects (Below 15nm Pitch)**: Future interconnects at these dimensions envision tall, barrier-less designs. This approach enhances electromigration (EM) reliability, ensuring durable and robust connections as dimensions continue to shrink, overcoming critical scaling challenges.

![Screenshot from 2024-11-26 00-36-54](https://github.com/user-attachments/assets/09ed8c46-a991-47ba-ad36-9069dfaa1eb6)

The image illustrates how a selective barrier, usually made of tantalum nitride (TaN), enhances copper interconnects in semiconductor manufacturing. This barrier minimizes resistance, prevents copper ion migration for improved reliability, and helps control copper thickness. The process includes cleaning the copper surface, depositing TaN through atomic layer deposition (ALD), and eliminating sacrificial layers, making it essential for reliable, high-performance semiconductor devices.


![Screenshot from 2024-11-26 00-37-14](https://github.com/user-attachments/assets/11910c01-0d6f-4862-acd6-aeed8dacf7dc)

Back-Side Power Delivery Network (BS-PDN)

Efficient power delivery is essential for the performance and reliability of integrated circuits in advanced semiconductor manufacturing. Traditional Front-Side Power Delivery Networks (FS-PDNs) often experience high IR-drop, which can hinder device performance and reliability. To overcome this, Back-Side Power Delivery Networks (BS-PDNs) have been introduced as an effective alternative.

BS-PDNs route power supply rails to the backside of the chip, allowing for shorter and wider power lines. This design significantly reduces IR-drop, enhancing power delivery efficiency. The benefits of BS-PDNs include:

- **Reduced IR-drop:** Lower voltage drops improve device performance and reliability.  
- **Smaller standard cell area:** More efficient power delivery supports smaller standard cell designs.  
- **Enhanced performance:** Reduced IR-drop enables faster switching speeds and minimizes power dissipation.  

By implementing BS-PDNs, semiconductor manufacturers can produce high-performance, energy-efficient integrated circuits to meet modern electronic demands.

## Installing and setting up ORFS

```bash
git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts
cd OpenROAD-flow-scripts
sudo ./setup.sh
```





</details>

