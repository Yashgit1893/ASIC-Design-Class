# Asic-design-class

<details>
<summary>LAB 1: C program to calculate sum from 1 to n (given n) and then compiling the code with both GCC compiler</summary>

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
<summary>LAB 2: Compiling the sum from 1 to n(lab 1) C program using RISC-V compiler. </summary>

Compiling the code using RISC-V compiler :

<p align="left">
  <img width="750" alt="Screenshot 2024-08-07 125754" src="https://github.com/user-attachments/assets/880561bb-45f1-466d-ada0-306014f6dbff">
</p>

compiling the `sum1ton.c` using the command :
`riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c`

<p align="left">
  <img width="750" alt="Screenshot 2024-08-07 124955" src="https://github.com/user-attachments/assets/b16030f7-9103-41bb-ac09-5b6a0fd19563">
</p>

compiling the `sum1ton.c` using the command ofast :
`riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c`

<p align="left">
  <img width="750" alt="2Screenshot 2024-08-07 125611" src="https://github.com/user-attachments/assets/52816414-52f2-49cd-b9e0-b25e3db9c375">
</p>
</details>



<details>
<summary>LAB 3: Compiling the sum from 1 to n(lab 1) C program using Spike simulator and debug the code. </summary>

compiling the `sum1ton.c` using the RISC-V compiler using the Ofast command :

```bash
riscv64-unknown-elf-gcc -ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c
```

<p align="left">
  <img width="750" alt="3_1" src="https://github.com/user-attachments/assets/9748f4ba-ca7f-4daf-af4d-09e8b7b6206f">
</p>

The above image shows the output using both `./a.out ` and `spike pk sum1ton.o`. Both of them have same output for sum from 1 to 100.

### Debug 

Debug the code using spike command :

```bash
spike -d pk sum1ton.o
```
<p align="left">
  <img width="437" alt="3_2" src="https://github.com/user-attachments/assets/f8219da7-f3e7-42a5-8a50-b5aa451ea796">
</p>

command for spike debugger to run till instruction 100b0
```bash
until pc 0 100b0
```
to check the value at the register a2
```bash
reg 0 a2
```
The image displays how the value of a2 register changes while manual debugging

<p align="left">
  <img width="440" alt="3_4" src="https://github.com/user-attachments/assets/4afaa25c-43f5-4e74-9997-76b20d08897c">
</p>

Futher steps shows the vlaue at register sp. we again run the instructions from 0 to 100b8.

```bash
until pc 0 100b8
```

check the value at the register sp
```bash
reg 0 sp
```

The below image shows the manual debug

<p align="left">
  <img width="440" alt="3_3" src="https://github.com/user-attachments/assets/4afaa25c-43f5-4e74-9997-76b20d08897c">
</p>

</details>
