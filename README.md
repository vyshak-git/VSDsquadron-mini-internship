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

To check if the output from the gcc matches with the output from the riscv gcc, we can use spike. To invoke it we do the following,
```bash
spike pk sum2n.o
```

The output: <br/>
![Screenshot from 2024-06-26 23-48-10](https://github.com/vyshak-git/VSDsquadron-mini-internship/assets/84836428/0a917510-8b10-42a2-b88e-f60aeff87bad) <br/>

The output is same hence the RISC-V instruction set is performing as expected. <br/>
We can enter the debug mode by giving a `-d` switch. This lets us explore the instructions step by step. <br/>
![Screenshot from 2024-06-26 23-50-36](https://github.com/vyshak-git/VSDsquadron-mini-internship/assets/84836428/97fd5849-cd77-42b2-9ebf-63cf5f6abe6f) 

Here `until pc 0 100b0` sets the pointer upto that address(100b0). So whatever registers data we access will be only upto that step. So when we give `reg 0 a2`, It outputs the data in the a2 register at that instance. When we press "Enter", the next instruction is executed and the data in register a2 changes. <br/>

## Project: Digital Clock Cycle Divider
A clock divider or a frequency divider is a digital circuit that takes an input frequency and puts out a reduced output frequency. <br/>
Let us write a simple C program to divide a clock frequency by a certain factor. <br/>
```C
#include <stdio.h>
#include <stdint.h>

#define DIVIDE_BY 2 //division factor

// Clock divider function
void clock_divider(uint32_t input_frequency, uint32_t divide_by) {
    uint32_t output_frequency = input_frequency / divide_by;

    printf("Input Frequency: %u Hz\n", input_frequency);
    printf("Divide by: %u\n", divide_by);
    printf("Output Frequency: %u Hz\n", output_frequency);
}

int main() {
//Input frequency = 100MHz
    uint32_t input_frequency = 100000000; 
    clock_divider(input_frequency, DIVIDE_BY);

    return 0;
}
```

The above code is very simple and divides the input frequency by the factor specified. <br/>
Here we use `uint32_t` which is a fixed-width integer type as we are writing the code for embedded application. <br/>
By changing the value of `DIVIDE_BY`, we can change the factor by which the frequency divides. <br/>

Let us compile it and look at the output: <br/>
![Screenshot from 2024-06-27 00-21-56](https://github.com/vyshak-git/VSDsquadron-mini-internship/assets/84836428/22273da1-dfee-41c9-918c-c957ec77940e) <br/>

So the frequency is being divided by 2 just like we intended. <br/>
We also compile it using the riscv gcc and verify the output using spike. <br/>
![Screenshot from 2024-06-27 00-35-34](https://github.com/vyshak-git/VSDsquadron-mini-internship/assets/84836428/a2f9ceac-d4f5-4849-8a97-98829e5a6d09)

We get the same output. <br/>

Let us take a look at the instruction set. We have compiled using both `O1` and `Ofast`. But in this program we observe that both of them have the same number of instructions for the main bloack unlike our previous program where there was a slight reduction in the `Ofast` compilation. The screenshots are shown below, <br/>

![Screenshot from 2024-06-27 00-39-37](https://github.com/vyshak-git/VSDsquadron-mini-internship/assets/84836428/365ac429-9ab6-4e0e-b3af-02e24102c6fc) <br/>
*compiled with O1*

![Screenshot from 2024-06-27 00-39-52](https://github.com/vyshak-git/VSDsquadron-mini-internship/assets/84836428/75e3ed9e-d7f4-4339-8331-8ce3f3dda9b4) <br/>
*compiled with Ofast*

## RISC-V Instruction types.
A CPU contains 32 general purpose registers indexed from 0-31. They are names X0-X31. The value of the first register (X0) is always 0. The rest 31 are readable/writable. In a 32-bit architecture, The width of each general purpose register is 32 bit. Similarly in a 64-bit architecture, the width is 64 bit. <br/>
The RISC-V instruction set has 6 formats. They are R,I,S,B,U,J. 
```bash
R -> reg 2 reg operations
I -> immediate & load operations
S -> store operation
B -> conditional branch operation
U -> long immediate
J -> unconditional jump.
```
The immage below shows the machine instruction format.
![word-image-1](https://github.com/vyshak-git/VSDsquadron-mini-internship/assets/84836428/58ffb7a7-1373-4087-8d01-3fcde0c70fef) <br/>
*Source: https://fraserinnovations.com/risc-v/risc-v-instruction-set-explanation/* <br/>
```bash
In the above image,
opcode -> Used to identify the type of instruction
rd -> destination register.
rs1 & rs2 -> source register.
funct3 & funct7 -> used to indentify the type of operation to be performed.
immediate -> integer value.
```

Let us look at the different instructions, <br/>
#### R-type 
These are operations wihtput an immediate.Immediate is a number that exists as an integer in the instructions. the values of rs1 & rs2 are operated upon and stored in rd. <br/>
Some of the R type instructions are ADD, SLT, SLTU, AND, OR, XOR, SLL, SRL, SRA, SUB.

#### I-type
These are instructions where the operation of the value in rs1 is performed with the immediate and the result is stored in rd. opcode is `OP-IMM == 7'b001_0011`. <br/>
Some of the instructions are ADDI, SLTI, SLTIU, ANDI, ORI, XORI.

#### U-type
LUI instruction sets immediate value to the high 20bits ofrd and 0 to the low 12bits. <br/>
AUIPC sets immadiate value to high 20bits of rd and adds the low 12 bits to the current PC. PC is program counter.








