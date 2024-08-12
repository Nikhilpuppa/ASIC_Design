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


### Instructions and Their Encodings:

#### 1. **`ADD r4, r4, r4`**
- **Type:** R
- **Opcode:** 0110011
- **rd:** 00010 (r4)
- **rs1:** 00010 (r4)
- **rs2:** 00010 (r4)
- **func3:** 000
- **func7:** 0000000
- **Instruction (Binary):** 0000000 00100 00100 000 00100 0110011
- **Hexadecimal:** 0x00420233

#### 2. **`SUB r4, r4, r4`**
- **Type:** R
- **Opcode:** 0110011
- **rd:** 00010 (r4)
- **rs1:** 00010 (r4)
- **rs2:** 00010 (r4)
- **func3:** 000
- **func7:** 0100000
- **Instruction (Binary):** 0100000 00100 00100 000 00100 0110011
- **Hexadecimal:** 0x40420233

#### 3. **`AND r4, r4, r4`**
- **Type:** R
- **Opcode:** 0110011
- **rd:** 00010 (r4)
- **rs1:** 00010 (r4)
- **rs2:** 00010 (r4)
- **func3:** 111
- **func7:** 0000000
- **Instruction (Binary):** 0000000 00100 00100 111 00100 0110011
- **Hexadecimal:** 0x00427233

#### 4. **`OR r8, r4, r5`**
- **Type:** R
- **Opcode:** 0110011
- **rd:** 01000 (r8)
- **rs1:** 00100 (r4)
- **rs2:** 00101 (r5)
- **func3:** 110
- **func7:** 0000000
- **Instruction (Binary):** 0000000 00101 00100 110 01000 0110011
- **Hexadecimal:** 0x00526433

#### 5. **`XOR r8, r4, r4`**
- **Type:** R
- **Opcode:** 0110011
- **rd:** 01000 (r8)
- **rs1:** 00100 (r4)
- **rs2:** 00100 (r4)
- **func3:** 100
- **func7:** 0000000
- **Instruction (Binary):** 0000000 00100 00100 100 01000 0110011
- **Hexadecimal:** 0x00424433

#### 6. **`SLT r00, r1, r4`**
- **Type:** R
- **Opcode:** 0110011
- **rd:** 00000 (r0)
- **rs1:** 00001 (r1)
- **rs2:** 00100 (r4)
- **func3:** 010
- **func7:** 0000000
- **Instruction (Binary):** 0000000 00100 00001 010 00000 0110011
- **Hexadecimal:** 0x0040A033  

#### 7. **`ADDI r02, r2, 5`**
- **Type:** I
- **Opcode:** 0010011
- **rd:** 00010 (r2)
- **rs1:** 00010 (r2)
- **imm:** 000000000101 (5)
- **func3:** 000
- **Instruction (Binary):** 000000000101 00010 000 00010 0010011
- **Hexadecimal:** 0x00510113

#### 8. **`SW r2, r0, 4`**
- **Type:** S
- **Opcode:** 0100011
- **rs1:** 00000 (r0)
- **rs2:** 00010 (r2)
- **imm:** 000000000100 (4)
- **func3:** 010
- **Instruction (Binary):** 000000000100 00000 010 00010 0100011
- **Hexadecimal:** 0x00402123

#### 9. **`SRL r06, r01, r1`**
- **Type:** R
- **Opcode:** 0110011
- **rd:** 00110 (r6)
- **rs1:** 00001 (r1)
- **rs2:** 00001 (r1)
- **func3:** 101
- **func7:** 0000000
- **Instruction (Binary):** 0000000 00001 00001 101 00110 0110011
- **Hexadecimal:** 0x0010D333

#### 10. **`BNE r0, r0, 20`**
- **Type:** B
- **Opcode:** 1100011
- **rs1:** 00000 (r0)
- **rs2:** 00000 (r0)
- **imm:** 00000000010100 (20)
- **func3:** 001
- **Instruction (Binary):** 000000010100 00000 001 00000 1100011
- **Hexadecimal:** 0x01401063

#### 11. **`BEQ r0, r0, 15`**
- **Type:** B
- **Opcode:** 1100011
- **rs1:** 00000 (r0)
- **rs2:** 00000 (r0)
- **imm:** 00000000001111 (15)
- **func3:** 000
- **Instruction (Binary):** 000000001111 00000 000 00000 1100011
- **Hexadecimal:** 0x00F00063

#### 12. **`LW r03, r01, 2`**
- **Type:** I
- **Opcode:** 0000011
- **rd:** 00011 (r3)
- **rs1:** 00001 (r1)
- **imm:** 000000000010 (2)
- **func3:** 010
- **Instruction (Binary):** 000000000010 00001 010 00011 0000011
- **Hexadecimal:** 0x0020A183

#### 13. **`SLL r05, r01, r1`**
- **Type:** R
- **Opcode:** 0110011
- **rd:** 00101 (r5)
- **rs1:** 00001 (r1)
- **rs2:** 00001 (r1)
- **func3:** 001
- **func7:** 0000000
- **Instruction (Binary):** 0000000 00001 00001 001 00101 0110011
- **Hexadecimal:** 0x001092B3
### RISC-V Instructions Summary

## Instructions and Their Encodings

| Instruction         | Type | 32-bit Instruction                             | Hexadecimal |
|---------------------|------|-----------------------------------------------|-------------|
| `ADD r4, r4, r4`   | R    | 0000000 00100 00100 000 00100 0110011       | 0x00420233  |
| `SUB r4, r4, r4`   | R    | 0100000 00100 00100 000 00100 0110011       | 0x40420233  |
| `AND r4, r4, r4`   | R    | 0000000 00100 00100 111 00100 0110011       | 0x00427233  |
| `OR r8, r4, r5`    | R    | 0000000 00101 00100 110 01000 0110011       | 0x00526433  |
| `XOR r8, r4, r4`   | R    | 0000000 00100 00100 100 01000 0110011       | 0x00424433  |
| `SLT r00, r1, r4`  | R    | 0000000 00100 00001 010 00000 0110011       | 0x0040A033  |
| `ADDI r02, r2, 5`  | I    | 000000000101 00010 000 00010 0010011       | 0x00510113  |
| `SW r2, r0, 4`     | S    | 000000000100 00000 010 00010 0100011       | 0x00402123  |
| `SRL r06, r01, r1` | R    | 0000000 00001 00001 101 00110 0110011       | 0x0010D333  |
| `BNE r0, r0, 20`   | B    | 000000010100 00000 001 00000 1100011       | 0x01401063  |
| `BEQ r0, r0, 15`   | B    | 000000001111 00000 000 00000 1100011       | 0x00F00063  |
| `LW r03, r01, 2`   | I    | 000000000010 00001 010 00011 0000011       | 0x0020A183  |
| `SLL r05, r01, r1` | R    | 0000000 00001 00001 001 00101 0110011       | 0x001092B3  |





</details>

<details><summary><strong>Lab4</strong></summary>
	
 ### Objective: 
  - Simulate the RISCV instruction
 ### Procdeure:
  - In the Verilog code, each instruction type is assigned a unique opcode, and the instructions have specific func3 and func7 values that differ from the original RISC-V specifications. The func7 value is used to differentiate between immediate operations and other arithmetic functions. If func7 is not utilized for this purpose, it is set to b'0 in the Verilog code.
  - This table contains the hard coded 32 bit instruction for the given RISCV instructions in Lab3
    

| Sno | Instruction         | Hardcoded 32-bit Pattern                  | Hexadecimal Pattern | 32-bit Pattern                        | Hexadecimal |
|-----|---------------------|-------------------------------------------|---------------------|---------------------------------------|-------------|
| 1   | `ADD r4, r4, r4`    | `0000001 00100 00100 000 00100 0000000`   | 0x02420200          | `0000000 00100 00100 000 00100 0110011` | 0x00420233  |
| 2   | `SUB r4, r4, r4`    | `0000001 00100 00100 001 00100 0000000`   | 0x02421200         | `0100000 00100 00100 000 00100 0110011` | 0x40420233  |
| 3   | `AND r4, r4, r4`    | `0000001 00100 00100 010 00100 0000000`   | 0x02422200         | `0000000 00100 00100 111 00100 0110011` | 0x00427233  |
| 4   | `OR r8, r4, r5`     | `0000001 00101 00100 011 01000 0000000`   | 0x02523400         | `0000000 00101 00100 110 01000 0110011` | 0x00526433  |
| 5   | `XOR r8, r4, r4`    | `0000001 00100 00100 100 01000 0000000`   | 0x02424400          | `0000000 00100 00100 100 01000 0110011` | 0x00424433  |
| 6   | `SLT r00, r1, r4`   | `0000001 00100 00001 101 00000 0000000`   | 0x0240D000        | `0000000 00100 00001 010 00000 0110011` | 0x00424433  |
| 7   | `ADDI r02, r2, 5`   | `000000000101 00010 000 00010 0000000`    | 0x00510100        | `000000000101 00010 000 00010 0010011`  | 0x00510113  |
| 8   | `SW r2, r0, 4`      | `000000 000100 00000 001 00010 0000001`   | 0x00401101          | `000000000100 00000 010 00010 0100011`  | 0x00402123  |
| 9   | `SRL r06, r01, r1`  | `0000000 00001 00001 001 00110 0000011`   | 0x00109303          | `0000000 00001 00001 101 00110 0110011` | 0x0010D333  |
| 10  | `BEQ r0, r0, 15`    | `000000001111 00000 000 00000 0000010`    | 0x00F00002          | `000000001111 00000 000 00000 1100011` | 0x00F00063  |
| 11  | `LW r03, r01, 2`    | `000000000010 00001 010 00011 0000011`    | 0x0020A183          | `000000000010 00001 010 00011 0000011`  | 0x0020A183  |

### Output WaveForm:

 1. ``` ADD r6, r1, r2 ```

![image](https://github.com/user-attachments/assets/cbf23082-1b95-4b73-93d2-1d89a50ed8e6)


2. ``` SUB r7, r1, r2  ```


 ![image](https://github.com/user-attachments/assets/97e14855-3f17-4f94-8ee1-970fe52b5e7d)
 

3.  ``` AND r8, r1, r3 ```
 

 ![image](https://github.com/user-attachments/assets/0a861382-5687-4349-9922-d2cc806ba511)
 

 4. ``` OR r9, r2, r5  ```
 
![image](https://github.com/user-attachments/assets/e09a563c-0268-4c2c-89fc-720e4c84519f)


5. ``` XOR r10, r1, r4 ```

![image](https://github.com/user-attachments/assets/a8a0ee27-6c15-4eb3-be80-a9b645637a92)


6. ``` SLT r11, r2, r4 ```

![image](https://github.com/user-attachments/assets/c37f11b7-103f-421f-9fc0-4b4389f42daf)

7. ``` ADDI r12, r4, 5 ```

![image](https://github.com/user-attachments/assets/defe4c4c-17fb-4dbd-b09f-7073727eaeea)

8. ``` SW r3, r1, 2 ```

![image](https://github.com/user-attachments/assets/88e47ae3-a8d3-4e70-b031-4f312a1c1d86)

9. ```  LW r0, r0, 15  ```

   ![image](https://github.com/user-attachments/assets/4fecc4b2-6f2f-4ac1-85e8-d9aea0acd91b)

10. ``` BEQ r0, r0, 15 ```

 ![image](https://github.com/user-attachments/assets/837fffa7-fba9-4039-96fe-9641fb0f7f34)
 

 11. ``` ADD r14, r2, r2 ```

 ![image](https://github.com/user-attachments/assets/d396a1c2-8c54-4f39-b107-29479d6cc627)

 
**Final Output**

![image](https://github.com/user-attachments/assets/a50a8aa6-151a-445c-a51f-561af167ae0a)














 



 







</details>
