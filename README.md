# Asic-design-class

<details>
<summary>LAB 1: C program to calculate sum from 1 to n (given n) and then compiling the code with both GCC and RISC-V compiler</summary>


`sum1ton.c` is the file containing code to calculate the sum from 1 to n.

<p align="left">
  <img width="750" alt="1ton" src="https://github.com/user-attachments/assets/c03a8f66-e356-447a-815a-be940fdeec59">
</p>

Compiling the code using GCC compiler :
compiling the `sum1ton.c` with `gcc sum1ton.c` and run the executable file `./a.out`

<p align="left">
  <img width="750" alt="10" src="https://github.com/user-attachments/assets/9512912e-08f9-4a01-8950-b18ee442cfa4">
</p>

Output for sum from 1 to 15 is shown.

<details>
<details>
<summary>LAB 2: C program to calculate sum from 1 to n (given n) and then compiling the code with both GCC and RISC-V compiler</summary>


Compiling the code using RISC-V compiler :

<p align="left">
  <img width="750" alt="Screenshot 2024-08-07 125754" src="https://github.com/user-attachments/assets/880561bb-45f1-466d-ada0-306014f6dbff">
</p>

compiling the `sum1ton.c` using the command :
`riscv64-unknown-elf-gcc -o1 -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c`

<p align="left">
  <img width="750" alt="Screenshot 2024-08-07 124955" src="https://github.com/user-attachments/assets/b16030f7-9103-41bb-ac09-5b6a0fd19563">
</p>

compiling the `sum1ton.c` using the command ofast :
`riscv64-unknown-elf-gcc -ofast -mabi=lp64 -march=rv64i -o sum1ton.o sum1ton.c`
<p align="left">
  <img width="750" alt="2Screenshot 2024-08-07 125611" src="https://github.com/user-attachments/assets/52816414-52f2-49cd-b9e0-b25e3db9c375">
</p>

</details>


