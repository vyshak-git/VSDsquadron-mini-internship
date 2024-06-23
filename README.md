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

The output is displayed successfully. The summation of 0 to 100 is 5050.


