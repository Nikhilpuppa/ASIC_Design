# ASIC_Design
<details><summary><strong>Lab1</strong></summary>

### Objective:
  - Compile and verify a simple C code (example: sum of n natural numbers) in both GCC and RISC-V GNU compiler tool chain on Ubuntu.
### Tools used:
  - GCC
  - RISC-V GNU complier.
### Methodolgy:
  #### Task_1 : 
  #### 1. Compile and verify simple C code in GCC environment.
  ##### Code:
  ```c
	#include <stdio.h>

	int main(){

		int sum=0;
		int n=50;
		for(int i=0;i<=n;i++){
			sum = sum + i;
		}
		printf("Sum of numbers from 1 to %d is %d\n",n,sum);
		return 0;
	}
```
#### 2. Commands used to compile and run the code:
  ```c
gcc 1tonsum.c
```
```
./a.out
```
#### 3. Output image:
We can see the sum of numbers from 1 to 50 is 1275
![image](https://github.com/user-attachments/assets/7b0ce050-98aa-4957-a5d1-e14883a4818c)
 #### Task_2: 
 #### Compile and verify using RISC-V GNU compiler and observe difference between Ofast and O1
 ##### O1 optimization:
 ##### 1. Commands for compiling O1 optimization:
 ```
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o 1tonsum.o 1tonsum.c
```
```
riscv64-unknown-elf-objdump -d s1tonsum.o | less
```
##### 2. Output image:
There are a total of 14 lines in the main section of O1 optimization.
![image](https://github.com/user-attachments/assets/c917ef07-74b3-4941-8c98-7d2e5a365a7b)


 ##### Ofast optimization:
 ##### 1. Commands for compiling Ofast optimization:
 ```
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o 1tonsum.o 1tonsum.c
```
```
riscv64-unknown-elf-objdump -d s1tonsum.o | less
```
##### 2. Output image:
There are a total of 11 lines in the main section of Ofast optimization.
![image](https://github.com/user-attachments/assets/63b63e54-3d8d-4b04-a695-ce6e43cc1e8d)

**As Ofast takes lesser number of instructions clearly Ofast is better than O1 optimization.**

</details>

<details><summary><strong>Lab2</strong></summary>
	
### Objective:
  - Compile the dump file and verify the output with GCC output of lab1
  - Debug the main section of the previously written C program and observe the reguster values sof each step.
	
### Tools used:
  - GCC
  - RISC-V GNU complier
  - Spike RISC-V Simualtor

### Methodolgy:
#### Task_1:
##### Command to compile the C code in RISCV Complier
 ```
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o 1tonsum.o 1tonsum.c
```
##### Execution of object file using Spike Simulator
Here in the below command, "pk" means proxy kernel.
```
spike pk 1tonsum.c
```
We can see that output of the code is still same as Lab1.
![image](https://github.com/user-attachments/assets/961598e5-cf1e-4450-9102-46c4200fbcf3)
#### Task_2:
##### Debugging using Spike Simulator
Spike Simulator is often used for debugging the object file.
```
spike -d pk s1tonsum.c
```
1. **We can start debugging from the instruction 100b0 (The start of the main function) by using the below command.**
	```
	until pc 0 100b0
	```
	![image](https://github.com/user-attachments/assets/df14801d-e49c-44e6-b86e-2cd159bd7c85)
2. **Check the contents in the registers**
	```
	reg 0 a0
	reg 0 sp
	```
	We can observe the intial values of the registers.
![image](https://github.com/user-attachments/assets/a721d221-b60c-4ce3-9e09-fd2a0e0459c8)

3. **Understanding lui instruction by checking the value in the register a0.**

	We can see that the a0 value is updated after executing lui(load upper immediate) .
The value 21(in hexadecimal) is given to the register a0.
Note that the lower 12 bits remains the same which means lui perfoms basic addition for upper 20 bits.
![image](https://github.com/user-attachments/assets/0b6e1405-b9a5-4b35-99eb-afc0ccca2a80)

4. **Understanding addi instruction by checking the value in the register sp.**

	We observe in the calculator app that the subtraction of the intial value and final value results in 16.
So we understand that addi instrcution adds -16 to the register sp.
![image](https://github.com/user-attachments/assets/b8e1ef93-ed46-4d34-b464-cfd8024315a5)


</details>

<details><summary><strong>Lab3</strong></summary>
	
### Objective:
  - To understand various RISC-V instruction types (R,I,S,B,U,J)
  - To determine the 32 bit instruction code for specific RISC-V instruction
### Theory:
 - RISC-V (Reduced Instruction Set Computer - V) is an open standard instruction set architecture (ISA) that is simple and modular. The RISC-V ISA is categorized into several types of instructions based on their functionality.
 - The six main instruction formats in RISC-V are R-Type, I-Type, S-Type, B-Type, U-Type, and J-Type. Each format is tailored for specific tasks such as arithmetic and logic operations, working with immediate values, branching, memory access, and jump instructions.
 - This image shows the instruction format for various RISC-V instructiopn types.
   ![image](https://github.com/user-attachments/assets/daad8ba5-5964-4cd9-9968-1be21f65fd71)
 - this image shows the base instruction set table for predeined values
 ![image](https://github.com/user-attachments/assets/ebfe1668-74d0-476a-a280-d34f80b4052d)
### Procedure:
- Given RISCV instructions are
  
```
 ADD r4, r4, r4
 SUB r4, r4, r4
 AND r4, r4, r4
 OR r8, r4, r5
 XOR r8, r4, r4
 SLT r00, r1, r4
 ADDI r02, r2, 5
 SW r2, r0, 4
 SRL r06, r01, r1
 BNE r0, r0, 20
 BEQ r0, r0, 15
 LW r03, r01, 2
 SLL r05, r01, r1 
```


### Instructions and Their Encodings

#### 1. **`ADD r4, r4, r4`**
- **Type:** R
- **Opcode:** 0110011
- **rd:** 00010 (r4)
- **rs1:** 00010 (r4)
- **rs2:** 00010 (r4)
- **func3:** 000
- **func7:** 0000000
- **Instruction (Binary):** 0000000 00100 00100 000 00100 0110011
- **Hexadecimal:** 0x002202B3

#### 2. **`SUB r4, r4, r4`**
- **Type:** R
- **Opcode:** 0110011
- **rd:** 00010 (r4)
- **rs1:** 00010 (r4)
- **rs2:** 00010 (r4)
- **func3:** 000
- **func7:** 0100000
- **Instruction (Binary):** 0100000 00100 00100 000 00100 0110011
- **Hexadecimal:** 0x402202B3

#### 3. **`AND r4, r4, r4`**
- **Type:** R
- **Opcode:** 0110011
- **rd:** 00010 (r4)
- **rs1:** 00010 (r4)
- **rs2:** 00010 (r4)
- **func3:** 111
- **func7:** 0000000
- **Instruction (Binary):** 0000000 00100 00100 111 00100 0110011
- **Hexadecimal:** 0x007202B3

#### 4. **`OR r8, r4, r5`**
- **Type:** R
- **Opcode:** 0110011
- **rd:** 01000 (r8)
- **rs1:** 00100 (r4)
- **rs2:** 00101 (r5)
- **func3:** 110
- **func7:** 0000000
- **Instruction (Binary):** 0000000 00101 00100 110 01000 0110011
- **Hexadecimal:** 0x005242B3

#### 5. **`XOR r8, r4, r4`**
- **Type:** R
- **Opcode:** 0110011
- **rd:** 01000 (r8)
- **rs1:** 00100 (r4)
- **rs2:** 00100 (r4)
- **func3:** 100
- **func7:** 0000000
- **Instruction (Binary):** 0000000 00100 00100 100 01000 0110011
- **Hexadecimal:** 0x004202B3

#### 6. **`SLT r00, r1, r4`**
- **Type:** R
- **Opcode:** 0110011
- **rd:** 00000 (r0)
- **rs1:** 00001 (r1)
- **rs2:** 00100 (r4)
- **func3:** 010
- **func7:** 0000000
- **Instruction (Binary):** 0000000 00100 00001 010 00000 0110011
- **Hexadecimal:** 0x00402033

#### 7. **`ADDI r02, r2, 5`**
- **Type:** I
- **Opcode:** 0010011
- **rd:** 00010 (r2)
- **rs1:** 00010 (r2)
- **imm:** 000000000101 (5)
- **func3:** 000
- **Instruction (Binary):** 000000000101 00010 000 00010 0010011
- **Hexadecimal:** 0x00510213

#### 8. **`SW r2, r0, 4`**
- **Type:** S
- **Opcode:** 0100011
- **rs1:** 00000 (r0)
- **rs2:** 00010 (r2)
- **imm:** 000000000100 (4)
- **func3:** 010
- **Instruction (Binary):** 000000000100 00000 010 00010 0100011
- **Hexadecimal:** 0x00400223

#### 9. **`SRL r06, r01, r1`**
- **Type:** R
- **Opcode:** 0110011
- **rd:** 00110 (r6)
- **rs1:** 00001 (r1)
- **rs2:** 00001 (r1)
- **func3:** 101
- **func7:** 0000000
- **Instruction (Binary):** 0000000 00001 00001 101 00110 0110011
- **Hexadecimal:** 0x001302B3

#### 10. **`BNE r0, r0, 20`**
- **Type:** B
- **Opcode:** 1100011
- **rs1:** 00000 (r0)
- **rs2:** 00000 (r0)
- **imm:** 00000000010100 (20)
- **func3:** 001
- **Instruction (Binary):** 00000000010100 00000 001 00000 1100011
- **Hexadecimal:** 0x01400063

#### 11. **`BEQ r0, r0, 15`**
- **Type:** B
- **Opcode:** 1100011
- **rs1:** 00000 (r0)
- **rs2:** 00000 (r0)
- **imm:** 00000000001111 (15)
- **func3:** 000
- **Instruction (Binary):** 00000000001111 00000 000 00000 1100011
- **Hexadecimal:** 0x00F00063

#### 12. **`LW r03, r01, 2`**
- **Type:** I
- **Opcode:** 0000011
- **rd:** 00011 (r3)
- **rs1:** 00001 (r1)
- **imm:** 000000000010 (2)
- **func3:** 010
- **Instruction (Binary):** 000000000010 00001 010 00011 0000011
- **Hexadecimal:** 0x00210283

#### 13. **`SLL r05, r01, r1`**
- **Type:** R
- **Opcode:** 0110011
- **rd:** 00101 (r5)
- **rs1:** 00001 (r1)
- **rs2:** 00001 (r1)
- **func3:** 001
- **func7:** 0000000
- **Instruction (Binary):** 0000000 00001 00001 001 00101 0110011
- **Hexadecimal:** 0x001282B3
## RISC-V Instructions Summary

| Instruction         | Type | 32-bit Pattern                          | Hexadecimal |
|---------------------|------|-----------------------------------------|-------------|
| `ADD r4, r4, r4`   | R    | `0000000 00100 00100 000 00100 0110011` | 0x002202B3  |
| `SUB r4, r4, r4`   | R    | `0100000 00100 00100 000 00100 0110011` | 0x402202B3  |
| `AND r4, r4, r4`   | R    | `0000000 00100 00100 111 00100 0110011` | 0x007202B3  |
| `OR r8, r4, r5`    | R    | `0000000 00101 00100 110 01000 0110011` | 0x005242B3  |
| `XOR r8, r4, r4`   | R    | `0000000 00100 00100 100 01000 0110011` | 0x004202B3  |
| `SLT r00, r1, r4`  | R    | `0000000 00100 00001 010 00000 0110011` | 0x00402033  |
| `ADDI r02, r2, 5`  | I    | `000000000101 00010 000 00010 0010011` | 0x00510213  |
| `SW r2, r0, 4`     | S    | `000000000100 00000 010 00010 0100011` | 0x00400223  |
| `SRL r06, r01, r1` | R    | `0000000 00001 00001 101 00110 0110011` | 0x001302B3  |
| `BNE r0, r0, 20`   | B    | `00000000010100 00000 001 00000 1100011` | 0x01400063  |
| `BEQ r0, r0, 15`   | B    | `00000000001111 00000 000 00000 1100011` | 0x00F00063  |
| `LW r03, r01, 2`   | I    | `000000000010 00001 010 00011 0000011` | 0x00210283  |
| `SLL r05, r01, r1` | R    | `0000000 00001 00001 001 00101 0110011` | 0x001282B3  |



</details>
