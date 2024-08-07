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

compiling the `sum1ton.c` for O1 optimization using the command :
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

compiling the `sum1ton.c` for Ofast optimization using the command :
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
The instruction `lui a0, 0x21` updates the a0 register from 0x0000000000001000 to 0x0000000000021000.




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

</details>
