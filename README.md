# VSDsquadron-mini-internship
The vsdsquadron mini is a RISC-V development board. It contains a 32-bit RISC-V core. It has 16KB of flash memory, 2KB SRAM and operates at 24Mhz. The board has 15 GPIO pins and supports protocols such as I2C, SPI, and USART. <br/>
In this internship we learn about the RISC-V architecturre and explore the capabilities of a RISC-V mircoprocessor using the vsdsquadron mini. We also build a project around the vsdsquadron mini.

### C code to RISC-V instruction set
In this section we'll be looking at how the C code will be represented as the instruction set for the RISC-V architecture. <br/>
Lets first test the GCC compiler. We write a code that counts the sum of numbers from 0 to n. We can use any text editor to write the C code but here I have used GVIM. We invoke it by,
```bash
gvim sum2n.c
```
Let us write a simple C code. <br/>
![Screenshot from 2024-06-22 17-38-10](https://github.com/vyshak-git/VSDsquadron-mini-internship/assets/84836428/0ca1984e-41a8-4999-a74d-04af4ededb2a) <br/>
Code is compiled using gcc compiler,
```bash
gcc sum2n.c
```
an output is generated `a.out`. Lets run it. <br>
![Screenshot from 2024-06-22 17-37-26](https://github.com/vyshak-git/VSDsquadron-mini-internship/assets/84836428/1169bd7d-96e7-4aff-a1a0-42be45e17eb7) <br/>
![Screenshot from 2024-06-22 17-37-49](https://github.com/vyshak-git/VSDsquadron-mini-internship/assets/84836428/d62ca931-5483-434f-bc50-80deaf0992b1)

The output is displayed successfully. The summation of 0 to 100 is 5050. <br/>

To convert our C code to the RISC-V instruction get we have to compile it differently.
```bash
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum2n.o sum2n.c
```
here, <br/>
lp64 -> long integer pointer 64bit <br/>
-o -> specifies output file <br/>
rv64 -> 64bit RISC-V <br/>
We run this command and obtain the output `sum2n.o`. <br/>
![Screenshot from 2024-06-23 12-44-40](https://github.com/vyshak-git/VSDsquadron-mini-internship/assets/84836428/b814dfe1-4c4b-4332-b7c9-bf0d6be2538b) <br/>

To view the output we run,
```bash
riscv64-unknown-elf-objdump -d sum2n.o | less
```
This opens the output in the less editor which makes it easier for navigation. <br/>
We look for the keyword main using `?main`. <br/>
![Screenshot from 2024-06-23 12-27-53](https://github.com/vyshak-git/VSDsquadron-mini-internship/assets/84836428/877c6644-2e96-4bfa-bb9c-8d1d56b42de8) <br/>

Let us count the number of instructions executed in the main block by using a hexadecimal calculator and subtracting the address numbers given at the left most end. <br/>
![Screenshot from 2024-06-23 12-33-57](https://github.com/vyshak-git/VSDsquadron-mini-internship/assets/84836428/d95730ec-91d1-4f5e-848c-1570e33d401d) <br/>

There are 15 lines of instructions in the main block.

Let us compile the code with a different approach. instead of `-O1`, lets use `-Ofast`. 
```bash
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum2n.o sum2n.c
```
Now lets look at the output and see the number of instructions again. <br/>
![Screenshot from 2024-06-23 12-37-56](https://github.com/vyshak-git/VSDsquadron-mini-internship/assets/84836428/c038bb67-75b7-412f-81d7-152d00c9928d) <br/>

We can see that the number of instructions have reduced and there are only 12 instructions.







