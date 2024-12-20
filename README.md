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

<details><summary><strong>Lab5</strong></summary>

### Objective:
  -  To verify that the output of the below code is same for both riscv-gcc and gcc compiler

### Code:
  - This function creates a simple dashboard to gte vehicle info. Also it contains functions to get and set values.

```c
#include <stdio.h>

void setEngineTemp(int temp);
int getEngineTemp();
void setFuelLevel(int level);
int getFuelLevel();
void setBatteryVoltage(float voltage);
float getBatteryVoltage();
void displayDashboard();


int engineTemp;
int fuelLevel;
float batteryVoltage;

int main() {
    int choice;
    int temp, level;
    float voltage;

    setEngineTemp(90);
    setFuelLevel(50);
    setBatteryVoltage(12.5);

    do {
        displayDashboard();
        printf("Enter your choice (1-7): ");
        scanf("%d", &choice);

        switch(choice) {
            case 1:
                printf("Enter new Engine Temperature (°C): ");
                scanf("%d", &temp);
                setEngineTemp(temp);
                printf("Engine Temperature updated to %d°C\n", getEngineTemp());
                break;

            case 2:
                printf("Current Engine Temperature: %d°C\n", getEngineTemp());
                break;

            case 3:
                printf("Enter new Fuel Level (%%): ");
                scanf("%d", &level);
                setFuelLevel(level);
                printf("Fuel Level updated to %d%%\n", getFuelLevel());
                break;

            case 4:
                printf("Current Fuel Level: %d%%\n", getFuelLevel());
                break;

            case 5:
                printf("Enter new Battery Voltage (V): ");
                scanf("%f", &voltage);
                setBatteryVoltage(voltage);
                printf("Battery Voltage updated to %.1f V\n", getBatteryVoltage());
                break;

            case 6:
                printf("Current Battery Voltage: %.1f V\n", getBatteryVoltage());
                break;

            case 7:
                printf("Exiting the dashboard...\n");
                break;

            default:
                printf("Invalid choice. Please select a valid option.\n");
        }
    } while(choice != 7);

    return 0;
}


void setEngineTemp(int temp) {
    engineTemp = temp;
}


int getEngineTemp() {
    return engineTemp;
}


void setFuelLevel(int level) {
    fuelLevel = level;
}


int getFuelLevel() {
    return fuelLevel;
}


void setBatteryVoltage(float voltage) {
    batteryVoltage = voltage;
}


float getBatteryVoltage() {
    return batteryVoltage;
}

void displayDashboard() {
    printf("\n--- Dashboard ---\n");
    printf("1. Set Engine Temperature\n");
    printf("2. Get Engine Temperature\n");
    printf("3. Set Fuel Level\n");
    printf("4. Get Fuel Level\n");
    printf("5. Set Battery Voltage\n");
    printf("6. Get Battery Voltage\n");
    printf("7. Exit\n");
}

```

### Compiling and Executing the code:

1) **In GCC**

 ``` 
 gcc vehicle_dashboard.c
 ```
 
 ``` 
 ./a.out
 ```

2) **In RISCV**


 ``` 
 riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o vehicle_dashboard.o vehicle_dashboard.c
```

```
spike pk vehicle_dashboard.o
```

### Code Output :

1) **In GCC**
   
   ![image](https://github.com/user-attachments/assets/3c05b5c1-c691-41b2-b905-f9e74aeb3f23)

2) **In RISCV**

   ![image](https://github.com/user-attachments/assets/5387375b-7757-410c-8e5b-a37362856bef)

## Conclusion:

 - We observe that the C code executes same in both GCC and RISCV .


</details>



<details><summary><strong>Lab6</strong></summary>
	
<details><summary><strong>Day3</strong></summary>
	
 ## Understanding Simple Combinational Circuit :
 
 - These are the basic gates used in combinational circuit.
 ![image](https://github.com/user-attachments/assets/4637de9e-4b64-4551-acf7-3690e3250874)

 - Here are some of the basic designs of some of the combinational ciruits:

 
 1) **Inverter**
 ```c
$out = ! $in;
```

![inverter](https://github.com/user-attachments/assets/8a4224d7-50fe-4261-a1d8-2b8d9d646a35)

 2) **Addition on arrays**
 ```c
$out[4:0] = $in1[3:0] + $in2[3:0];
```

![adder](https://github.com/user-attachments/assets/22698417-c934-42b6-8c1a-cd2435543bf6)

 3) **And Gate(2 input)**
 ```c
$out = $in1 && $in2;
```

![ans](https://github.com/user-attachments/assets/ba5aaafe-4994-4269-b98e-92cc141d639d)

 4) **Or Gate(2 input)**
 ```c
$out = $in1||  $in2;
```
![or](https://github.com/user-attachments/assets/fe2cc74a-77b3-415a-9036-2d863ed372b8)


 5) **xor Gate(2 input)**
 ```c
$out = $in1 ^  $in2;
```
![xor](https://github.com/user-attachments/assets/e942922b-faea-4523-8ca7-272ccaa6d3b7)


 6) **2:1 Mux**
 ```c
$out[11:0] = $sel ? $in1[11:0] : $in0[11:0];
```

![mux21](https://github.com/user-attachments/assets/8fa6d6e8-f7ef-40a4-b45d-07b8e376304b)


 7) **Combinational Calci**

- This calculator does 4 basic operations which are addition, subtraction,divison and multiplication.
- The input to the calculator is random 3 bit values and the operation is selected by the select line of the mux.
 ```c
$val1[31:0] = $rand1[3:0];
$val2[31:0] = $rand2[3:0];

$sum[31:0]  = $val1[31:0] + $val2[31:0];
$diff[31:0] = $val1[31:0] - $val2[31:0];
$prod[31:0] = $val1[31:0] * $val2[31:0];
$quot[31:0] = $val1[31:0] / $val2[31:0];

$out[31:0] = ($sel[1:0] == 2'b00) ? $sum[31:0]:
             ($sel[1:0] == 2'b01) ? $diff[31:0]:
             ($sel[1:0] == 2'b10) ? $prod[31:0]:
                                    $quot[31:0];
```

- If select line is 00 then additon is done
- If select line is 01 then subtraction is done
- If select line is 10 then multiplication is done
- If select line is 11 then division is done

- Later in the editoral we see that this calaculator can be implemented by sequenctial logic too.

![calci](https://github.com/user-attachments/assets/e6afbcf5-7bbb-44b7-8234-93f10ae8cb31)


## Understanding Sequential Circuit :

- A sequential circuit is a type of digital circuit that incorporates memory components to store data, enabling it to produce outputs based on both the current inputs and the circuit's past states. Unlike combinational circuits, which rely solely on present inputs, sequential circuits utilize feedback loops and storage elements like flip-flops or registers to maintain their internal state. This internal state, combined with current inputs, determines the circuit's behavior, allowing it to perform tasks that require tracking input history, such as counting, data storage, or event sequencing.

1) **Fibbonacci Series**:

``` c
$num[31:0] = $reset ? 1 : (>>1$num + >>2$num);
```
- The below image shows the circuit consisting of d flip flop
![image](https://github.com/user-attachments/assets/ad4f9937-e543-499e-bc20-aeeffaebdfe9)


![fib](https://github.com/user-attachments/assets/455b313c-a04b-43d5-bb94-e8ee0f9a258d)


2) **Counter Series**:

```c
$cnt[31:0] = $reset ? 0 : (>>1$cnt + 1);
```

- The below image shows the circuit consisting of d flip flop

 ![image](https://github.com/user-attachments/assets/95540797-cb40-433e-bd88-0a7d78b8737a)


![cnt](https://github.com/user-attachments/assets/9777d78f-2421-4e76-b3cf-6ecd5d8a98d3)


3) **Sequential Calculator:**
- This circuit behaves the same as the rpevious cominitoral circuit but the result will the input to the circuit .
-  Upon resest the result sets to zero.
-  
```c

   $val1[31:0] = >>2$out;
   $val2[31:0] = $rand2[3:0];

   $sum[31:0]  = $val1[31:0] + $val2[31:0];
   $diff[31:0] = $val1[31:0] - $val2[31:0];
   $prod[31:0] = $val1[31:0] * $val2[31:0];
   $quot[31:0] = $val1[31:0] / $val2[31:0];

   $nxt[31:0] = ($sel[1:0] == 2'b00) ? $sum[31:0]:
                ($sel[1:0] == 2'b01) ? $diff[31:0]:
                ($sel[1:0] == 2'b10) ? $prod[31:0]:
                                       $quot[31:0];
   
   $out[31:0] = $reset ? 32'h0 : $nxt;
```

- The generated block diagram and waveforms are as shown:

![image](https://github.com/user-attachments/assets/fad11b10-4f92-4cc0-85e5-5882ff17e477)

![calci_sseq](https://github.com/user-attachments/assets/793f447d-bff7-42c2-bcb7-a2b5fc8ef928)


## Pipelined Logic:
 - In Transaction-Level Verilog (TL-Verilog), pipelined logic is effectively modeled using pipeline constructs that represent the flow of data through various design stages, each aligned with a clock cycle. This method simplifies the representation of sequential logic by automatically managing state propagation, allowing for clear and concise descriptions of complex, multi-stage operations. As a result, it enhances both the clarity and maintainability of the design.

1)**Designing for the given modele**:
   
- The objective is to recreate the design using TVL code. The module is shown in the diagram.
![image](https://github.com/user-attachments/assets/4c93ac77-6a26-4047-87cf-903eae14ea94)
 - Code for the following module is given below
   
 ```c
|pipe
  @1
    $err1 = $bad_input || $illegal_op;
  @3
    $err2 = $over_flow || $err1;
  @6
    $err3 = $div_by_zero || $err2;
```

- The results can be verified by seeing the below waveforms.
  ![failed](https://github.com/user-attachments/assets/e30db172-25e2-4b5e-8ef7-eebe0988f242)

2)**Pipelined Calculator**:
- Valid is also used to create alternate values of reset.

```c
   |cal
      @1
         $reset = *reset;
         $clk_nik = *clk;

         $valid[31:0] = $reset ? 0 : (>>1$valid + 1);
         $nreset = $reset | ~$valid;
         
         $val1[31:0] = >>2$out;
         $val2[31:0] = $rand2[3:0];

         $sum[31:0]  = $val1[31:0] + $val2[31:0];
         $diff[31:0] = $val1[31:0] - $val2[31:0];
         $prod[31:0] = $val1[31:0] * $val2[31:0];
         $quot[31:0] = $val1[31:0] / $val2[31:0];
         
      @2
         $nxt[31:0] = ($sel[1:0] == 2'b00) ? $sum[31:0]:
                      ($sel[1:0] == 2'b01) ? $diff[31:0]:
                      ($sel[1:0] == 2'b10) ? $prod[31:0]:
                                             $quot[31:0];
        
         
         $out[31:0] = $nreset ? 32'h0 : $nxt;   
   ```
![image](https://github.com/user-attachments/assets/686e1ca7-4438-42aa-86eb-2606269f050f)

![cal2](https://github.com/user-attachments/assets/2648f019-65f9-4f23-b192-af8474ad53c5)


3)**Cycle Calculator with validity:**:


```c
   $reset = *reset;
   $clk_nik = *clk;
   
   |cal
      @1
         $reset = *reset;
         $clk_kar = *clk;
         
         $cnt[31:0] = $reset ? 0 : (>>1$cnt + 1);
         $valid = $cnt;
         $valid_or_reset = ($reset | $valid);
         
      
      ?$valid
         @1
            $val1[31:0] = >>2$out;
            $val2[31:0] = $rand2[3:0];
            
            $sum[31:0]  = $val1[31:0] + $val2[31:0];
            $diff[31:0] = $val1[31:0] - $val2[31:0];
            $prod[31:0] = $val1[31:0] * $val2[31:0];
            $quot[31:0] = $val1[31:0] / $val2[31:0];
            
         @2
            $nxt[31:0] = ($sel[1:0] == 2'b00) ? $sum[31:0]:
                         ($sel[1:0] == 2'b01) ? $diff[31:0]:
                         ($sel[1:0] == 2'b10) ? $prod[31:0]:
                                                $quot[31:0];
            
            $out[31:0] = $valid_or_reset ? 32'h0 : $nxt;

```

![image](https://github.com/user-attachments/assets/b98d65db-d374-42ad-a783-ce6bc55dc519)

 </details>
 
 <details><summary><strong>Day4</strong></summary>
	 
  ## RISC-V CPU Micro-architecture:
  - This section contains the TVL implementation of 3 stages : Fetch, Decode, Execute
  - The below figure shows block diagram of our CPU.
    ![image](https://github.com/user-attachments/assets/ea49e898-8d50-44be-a99d-a72f7323ccf6)

  - We implement every block in the above diagram in TVL and show the resultant wave forms.
    ![image](https://github.com/user-attachments/assets/20aec983-a05f-4075-ba39-bda56f2af35c)

    **1) Program Counter**:
    
   	- In RISC-V, the Program Counter (PC) is a register that holds the address of the next instruction to be executed. After each instruction fetch, the PC is automatically incremented to point to the 		subsequent instruction. The PC can also be modified by control flow instructions like branches or jumps, directing the flow of program execution.
    	- The diagram below shows the diagram of PC in detail.
   	![image](https://github.com/user-attachments/assets/2d717fa8-264b-4621-8d16-87db6f64b17d)
    	- Given beloww is the TVL code for PC.
      	```c
      	$pc[31:0] = >>1$reset ? 0 : ( >>1$pc + 31'h4 );
      	```
       
      ![image](https://github.com/user-attachments/assets/65ca803d-4230-40a7-8dbd-41bf08cc8f17)



    **2) Instruction Memory**:
    
    - Given below is the low level diagram for the Instruction Memory.
    
	 ![image](https://github.com/user-attachments/assets/1179c300-15da-4021-a2d5-5f573ca72fb0)
	- The PC output is given to instruction memory which fetches the untruction which has to executed.
    	- The instruction memory gives the 32 bit instruction which needs to be exectuted to the next module whicle PC increments by 4.
        - The code for instruction memory is given below.
        ```c
        $imem_rd_en = >>1$reset ? 0 : 1;
        $imem_rd_addr[31:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
        $instr[31:0] = $imem_rd_data[31:0];
        ```
   	- Given below is the waveform for PC and instruction memory.

     ![image](https://github.com/user-attachments/assets/151476c8-5597-4412-813d-89c3c3f4297d)


    **3) Decoding the instruction**:

     **Decode Logic**:
     - Given below is the different instruction types which determines the type(I,R,S,B,J,U).
   	 ![image](https://github.com/user-attachments/assets/d56401fc-2bed-48e8-a5a6-d1d3cef981b1)

	 ![image](https://github.com/user-attachments/assets/d7a7d95d-97e3-470f-8728-0fe011a19b6d)

     **Immediate Logic**:

     - The figure below shows the immediate value of different instructions.
    
	![image](https://github.com/user-attachments/assets/e51fd527-0b69-401d-95a7-02e8ee143067)
     
     - Coded the instruction baseed on all six types of RISCV instruction set.
       
       ![image](https://github.com/user-attachments/assets/c552ed06-1999-4bf2-8801-23b18fea2a27)

    - The waveforms for the same are shown
      ![image](https://github.com/user-attachments/assets/d2335d14-2935-4947-b297-bc7b4d38fbd1)


      **Decode Logic For Other Fields**:
      
      - Functions like rs1,rs2,func3,func7 should also be decoded
        
        ```c
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
        ```
      - The waveform for the above is shown in the picture

        ![image](https://github.com/user-attachments/assets/b4af4364-65b4-42d2-8dd5-b9c66e996da5)

    **Decoding Individual Instructions**:
    - The task is the decode the circled individual instructions. This achieved by using the below code
      ```c
      $dec_bits [10:0] = {$funct7[5], $funct3, $opcode};
      $is_beq = $dec_bits ==? 11'bx_000_1100011;
      $is_bne = $dec_bits ==? 11'bx_001_1100011;
      $is_blt = $dec_bits ==? 11'bx_100_1100011;
      $is_bge = $dec_bits ==? 11'bx_101_1100011;
      $is_bltu = $dec_bits ==? 11'bx_110_1100011;
      $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
      $is_addi = $dec_bits ==? 11'bx_000_0010011;
      $is_add = $dec_bits ==? 11'b0_000_0110011;
      ```
    - Also the PC should be updated in order to account Branch instructions.
      ```c
      $pc[31:0] = >>1$reset ? 32'b0 :
            >>1$taken_branch ? >>1$br_target_pc :
            >>1$pc + 32'd4;
      ```
    - The waveforms are attached below
      ![image](https://github.com/user-attachments/assets/2529f7f2-f3af-4c5c-b021-3224d2cd5700)

    **4)Register File Read and Enable**
    - In this stage we read instructons from instruction memory and store them in registers.
    - We have two register slots that read the instructions, and these stored instructions are then sent to the ALU for processing.
      
      ![image](https://github.com/user-attachments/assets/1d345e3e-fa9c-48d8-a508-a97438aae225)

    ```c
    $rf_rd_en1 = $rs1_valid;
    $rf_rd_en2 = $rs2_valid;
    $rf_rd_index1[4:0] = $rs1;
    $rf_rd_index2[4:0] = $rs2;
    $src1_value[31:0] = $rf_rd_data1;
    $src2_value[31:0] = $rf_rd_data2;  
    ```

   - The waveform for the same is shown below
    ![image](https://github.com/user-attachments/assets/fd998880-8f81-404d-8354-8f2c49f26a0a)

   **5)ALU**
    ![image](https://github.com/user-attachments/assets/bff09f8b-c0ef-435d-9411-64a2a27cbe72)
    
   - This snippet accounts for addi and add operations.
   ```c
   $result[31:0] = $is_addi ? $src1_value + $imm :
                $is_add ? $src1_value + $src2_value :
                32'bx ;
   ```
   ![image](https://github.com/user-attachments/assets/4a35daf0-0dee-4f25-856c-e5240e679374)


   **6)Register File write**
   
   ![image](https://github.com/user-attachments/assets/5ecb817f-bbb3-444f-a4e6-bd97565a5c4e)

   ```c
   $rf_wr_en = $rd_valid && $rd != 5'b0;
   $rf_wr_index[4:0] = $rd;
   $rf_wr_data[31:0] = $result;
   ```
  ![image](https://github.com/user-attachments/assets/4c3c5d1b-50c8-4658-98d7-d3127698e41e)


  **7)Branch Instructions**
  ![image](https://github.com/user-attachments/assets/d6b8811f-c225-4b7c-8f01-115be7baa298)
  
  ```c
  $taken_branch = $is_beq ? ($src1_value == $src2_value):
                $is_bne ? ($src1_value != $src2_value):
                $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                $is_bltu ? ($src1_value < $src2_value):
                $is_bgeu ? ($src1_value >= $src2_value):1'b0;

 $br_target_pc[31:0] = $pc +$imm;
 ```
![image](https://github.com/user-attachments/assets/f8df5ed5-0864-4b90-ab1e-7019757d0699)


  **Test Bench**
  
  ```c
  *passed = |cpu/xreg[10]>>5$value == (1+2+3+4+5+6+7+8+9) ;
  ```

 ![image](https://github.com/user-attachments/assets/45a1f671-9342-4f18-ac81-cd9410b9314d)

**Final Result showing addition from 1 to 9 0(h00) to 45(h2d)**:
 ![image](https://github.com/user-attachments/assets/b8ed872c-3710-44ec-a106-bf33889dfe6b)


 </details>
 
<details><summary><strong>Day5</strong></summary>
	
## RISCV CPU:
**Pipelining the CPU**:

- Pipelining can introduce various hazards, one of the most significant being the "branch instruction hazard" or "branch penalty." This occurs because branch instructions can alter the flow of execution, leading to uncertainties. The main types of hazards include:

 1) Structural Hazard: This arises from resource conflicts, such as when multiple instructions need the same execution unit simultaneously, causing pipeline stalls until the conflict is resolved.
 2) Data Hazard: This occurs when an instruction depends on the result of a previous instruction. If the required data isn't available in time, it can lead to incorrect outcomes.
 3) Control Hazard (Branch Hazard): This is due to the uncertainty of branch outcomes, where the correct instruction to fetch isn't confirmed until after execution. This can result in fetching incorrect instructions and performance penalties due to pipeline flushing.

**Including Valid Signal**:
```c
$start = >>1$reset && !$reset;
$valid = $reset ? 1'b0 : ($start || >>3$valid);
$valid_or_reset = $valid || $reset;
$rs1_or_funct3_valid    = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
$rs2_valid              = $is_r_instr || $is_s_instr || $is_b_instr;
$rd_valid               = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
$funct7_valid           = $is_r_instr;
```
**ByPassing**:
```c
$src1_value[31:0] = $rs1_bypass ? >>1$result[31:0] : $rf_rd_data1[31:0];
$src2_value[31:0] = $rs2_bypass ? >>1$result[31:0] : $rf_rd_data2[31:0];
```
**Correction in branch target path**:
```c
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
```
**Final 5 Stage Pipeline:**
```c
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
         $clk_nik = *clk;
         
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


**Block Diagram**:

![image](https://github.com/user-attachments/assets/d1ecabf7-e215-46d6-aea1-f2d7f5bf24e0)


**WaveForm**:

**1)CLC:**

![image](https://github.com/user-attachments/assets/b5c0470e-1775-4526-8fc8-6431ceef1915)

**2)RESET**

![image](https://github.com/user-attachments/assets/aa92e4cf-dc59-4b6a-9a63-948128052ac0)

**3)increment of output from 0(h00) to 45(h2d)**:

![image](https://github.com/user-attachments/assets/f64dc1fb-f222-4161-9e35-eec30cb4a718)

**4)VIZ**:

![image](https://github.com/user-attachments/assets/b98a7c15-91ce-4c85-b3f0-74a6c83b6fb7)



</details>
</details>


<details><summary><strong>Lab7</strong></summary>
	
## Comparision of RISC-V Pre-Synthesis Simulation outputs using Iverilog GTKwave and Makerchip

- The RISC-V processor was designed using TL-Verilog in the Makerchip IDE. To implement it on an FPGA, it was converted to Verilog using the Sandpiper-SaaS compiler. Pre-synthesis simulations were then conducted using the GTKWave simulation.

## Procedure:
 1) Execute the following commands to set up a development environment for working with simulation and synthesis tools, specifically for tasks involving Verilog and RISC-V.
 ```c
$ sudo apt install make python python3 python3-pip git iverilog gtkwave

$ cd ~

$ sudo apt-get install python3-venv

$ python3 -m venv .venv

$ source ~/.venv/bin/activate

$ pip3 install pyyaml click sandpiper-saas
```

![image](https://github.com/user-attachments/assets/0872d51b-eea0-4116-853c-c2342c35d279)

 2)To install packages execite the following commands in virtual environment.
 
 ```c
$ sudo apt install make python python3 python3-pip git iverilog gtkwave docker.io

$ sudo chmod 666 /var/run/docker.sock

$ cd ~

$ pip3 install pyyaml click sandpiper-saas
```
![image](https://github.com/user-attachments/assets/443acadf-632b-43f2-a730-7aa6e8de9e7e)

3)Clone the repo in home directory and make a `pre_synth_sim` directory which contains the output.
```c
$ cd ~

$ git clone https://github.com/manili/VSDBabySoC.git

$ cd /home/vsduser/VSDBabySoC

$ make pre_synth_sim
```
![image](https://github.com/user-attachments/assets/14b4ef65-d3d1-4d5d-b2e3-f5755c85662a)

4)Replace the rvmyth.tlv file in the VSDBabySoC/src/module folder with your RISC-V design from the Makerchip .tlv file. Additionally, update the testbench to align with your Makerchip code.

5)To convert TLV code to verilog code use the below code.
```c
$ iverilog -o output/pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module
```
![image](https://github.com/user-attachments/assets/a6a59183-627b-4647-923e-35a8d5686922)

6)Compile and Run RISCV design
```c
$ iverilog -o output/pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module
```

7)Result is stored in ./pre_synth_sim.out.
![image](https://github.com/user-attachments/assets/1f345a78-ede1-4e47-a800-b7e3bc0edc30)

8)To view the simulation run the3 folllowing command.
```c
$ gtkwave pre_synth_sim.vcd
```

**Pre-synthesis Simulation results**: Signals to plot are the following:

 -clk_nik: This is the clock input to the RISC-V core.

-reset: This is the input reset signal to the RISC-V core.

-OUT[9:0]: Register 14 is output here.


**GTKWAVE Wave Forms**:

- clk_nik :
![WhatsApp Image 2024-08-26 at 16 49 21_7c09e66e](https://github.com/user-attachments/assets/7cbb801d-38cb-40ae-9e2b-fbe854ab9947)

- reset:
![WhatsApp Image 2024-08-26 at 16 49 47_97660766](https://github.com/user-attachments/assets/78bbd0e6-2c11-434a-9bf4-f67d1c2583cd)

- out[9:0]:
![WhatsApp Image 2024-08-26 at 17 06 00_22384aef](https://github.com/user-attachments/assets/489b750b-f0ab-4bcd-a7ce-4f777beb42d6)



**Makerchip results**:

- clk_nik :
![image](https://github.com/user-attachments/assets/43cb6b3f-15a3-43c4-818a-0b8030ac9d95)


- reset:
![image](https://github.com/user-attachments/assets/97c120c4-19e1-49f2-a1c9-1f72f1223e28)


- out[9:0]:
![image](https://github.com/user-attachments/assets/eaa7ff3f-a9de-41c8-bb48-2893524988d2)


</details>




<details><summary><strong>Lab8</strong></summary>
	
## AIM:
The task involves integrating and validating the functionality of a custom RISC-V processor (rvmyth) within the BabySoC platform. This will be done using a specific set of digital design and simulation tools to generate DAC and PPL waveforms for the RISC-V processor.

## Phase Locked Loop (PLL):

**Overview of PLL**: A Phase-Locked Loop (PLL) is an electronic control system designed to generate an output signal whose phase is precisely synchronized with the phase of an input signal. This synchronization allows the PLL to lock onto the frequency of the input signal.

**Applications**: PLLs are widely used in telecommunications, radio, and computing. They are crucial for achieving precise signal synchronization, maintaining frequency stability, and generating accurate clocks for digital circuits.

**Functionality and Benefits**: By adjusting its internal parameters, a PLL can stabilize and filter signals, ensuring reliable performance across various frequencies and applications. This capability makes it an essential tool for accurate signal processing and frequency management.

## PROCEDURE:
Use the following command to download files of BabySoc.
```c
git clone https://github.com/manili/VSDBabySoC.git
```
![image](https://github.com/user-attachments/assets/2125876e-3cb9-43d8-87ba-5e62b9fb758d)


**Verilog Code**

![image](https://github.com/user-attachments/assets/6d7e56d2-476c-46fb-a44f-f6088f248079)


## SIMULATION:
Functional simulation can be performed using the following command:
```c
cd BabySoC_Simulation
iverilog -o ./pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module/
./pre_synth_sim.out
gtkwave pre_synth_sim.vcd

```
### WAVEFORMS:

**clk**:

![image](https://github.com/user-attachments/assets/a76840e3-1235-4f94-8547-a0bebc5a5845)

**reset**:

![image](https://github.com/user-attachments/assets/7d42a79f-bc6c-4e77-9ea8-4abeec31dd63)

**output**:

![image](https://github.com/user-attachments/assets/f15fbafe-41d8-4d52-a7f9-9a792778daf3)


</details>

<details><summary><strong>Lab9</strong></summary>

<details><summary><strong>Day1</strong></summary>
	
## Introduction to Icarus Verilog (iverilog) : 

In digital circuit design, **Register-Transfer Level (RTL)** is an abstraction used to model synchronous digital circuits. RTL describes the flow of data between hardware registers and the logic operations applied to these signals. This abstraction is expressed using **Hardware Description Language (HDL)**, such as Verilog, to create high-level models. These models are eventually transformed into lower-level representations and, finally, into the physical hardware design.

### Simulator

A simulator is a tool used to verify the circuit design. In this context, we use the **Icarus Verilog (iverilog)** tool for simulation. Simulation involves creating models that mimic the behavior of the target device (simulation models) and using test models (test benches) to validate the device's design. RTL design typically consists of one or more Verilog files that specify the functionality and requirements of the system.

### Test Bench

A **test bench** is a setup that delivers stimulus (test vectors) to the design, verifying its behavior and ensuring that it meets the required specifications.

### Simulator

The simulator monitors input signals for changes, and based on those changes, evaluates the corresponding output.

![image](https://github.com/user-attachments/assets/7912ec0a-847e-4e38-92ef-0844eb31c1a0)

The design may have one or more primary inputs and primary outputs, but the test bench does not.

![image](https://github.com/user-attachments/assets/fb3cad13-a612-4684-bb85-9f503da438bb)

## LABS:

### Environmental setup : 
 ```
mkdir VLSI

cd VLSI

git clone https://github.com/kunalg123/vsdflow.git

git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git

cd sky130RTLDesignAndSynthesisWorkshop
```

![image](https://github.com/user-attachments/assets/ce597b89-a967-429e-a6a2-5bc4a778cb9e)

![image](https://github.com/user-attachments/assets/4aa76402-24bd-4f3b-96b3-d90e965cbf7f)

### Simulation of 2:1 multiplexer rtl design
 - Use the following commands to run a simple 2:1 mux
```
iverilog good_mux.v tb_good_mux.v

./a.out

gtkwave tb_good_mux.vcd

```
![image](https://github.com/user-attachments/assets/44631145-17fe-4958-a240-e64707759faf)

![image](https://github.com/user-attachments/assets/b33cfc44-bd9a-4df4-bd6e-5e69c27cbe08)

![image](https://github.com/user-attachments/assets/8e30de81-317b-4966-b53d-a451ed2c7aa9)

 - Use the following commands to access the contents in good_mux file

```
 vim tb_good_mux.v -o good_mux.v
```
![image](https://github.com/user-attachments/assets/10f42970-e496-46d1-94cb-1d781c657f63)

![image](https://github.com/user-attachments/assets/f9aa4240-60e5-40af-9bd2-9847907efd4a)

## Introduction to Yosys & Logic Synthesis:

- A synthesizer is a tool used to convert RTL designs into a netlist, and in this case, we are using the Yosys synthesizer.

![image](https://github.com/user-attachments/assets/e2f75528-4e0b-48cd-84c6-d67cf1607f06)

![image](https://github.com/user-attachments/assets/4ba80cb9-ae7e-4010-aafc-5dc35d5bd518)

- The set of primary inputs and primary outputs remains unchanged between the RTL design and synthesis, allowing us to use the same testbench for both.

### Logic Synthesis:

## RTL Design

The **Register-Transfer Level (RTL) design** is a behavioral representation written in HDL (Hardware Description Language) to define the functionality based on the required specifications.

## Synthesis

**Synthesis** is the process of translating the RTL design into a gate-level representation. During this step, the design is transformed into logic gates, and the necessary connections are established. The output of this process is a file known as a **netlist**.

## Liberty Files (.lib)

The **Liberty (.lib) file** is a collection of logical modules that includes basic logic gates such as AND, OR, and NOT. These gates are available in various configurations, including 2-input, 3-input, and 4-input versions. Additionally, the library provides multiple variants of each gate, ranging from slow to medium to fast.

- **Fast cells** are typically used to meet high-performance requirements but come at the cost of increased area and power consumption, and they may introduce hold time violations.
- **Slower cells** help to address hold time issues but, if overused, can negatively impact overall performance.

Optimal cell selection during synthesis is based on specific constraints that aim to balance area, power, and timing requirements.


![image](https://github.com/user-attachments/assets/1953f822-6848-4752-8a4c-f42a8ec0bbad)

## Faster vs. Slower Cells

### Impact of Cell Delay

The **cell delay** in digital logic circuits is largely influenced by the circuit’s load, typically represented by capacitance. Faster charging or discharging of this capacitance results in reduced cell delay.

### Wider Transistors for Faster Cells

To decrease cell delay, **wider transistors** are used as they can supply more current, enabling quicker charging or discharging of the capacitance. While this reduces the delay, it also increases **power consumption** and the **area** required for the circuit.

### Narrower Transistors for Slower Cells

In contrast, **narrower transistors** consume less area and power, but this comes at the cost of higher cell delay due to the slower charging/discharging process.

### Trade-off in Design

Achieving a low cell delay in a design requires balancing **area, power,** and **performance**. Faster cells improve performance but demand more area and power, while slower cells are more power-efficient and compact but can degrade the overall speed.


![image](https://github.com/user-attachments/assets/352d0677-b505-4e70-ae7c-b1c9c2033789)


### Ysoys Flow:

- Starting yosys.

```
yosys
```
![image](https://github.com/user-attachments/assets/4a877815-b086-4bec-9a40-77400938e4a1)



- Loading sky130 standard library
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
![image](https://github.com/user-attachments/assets/590b7ec1-3196-437f-ae36-f4a9020097c3)



- Reading the design files
```
read_verilog good_mux.v
```
![image](https://github.com/user-attachments/assets/e01be841-67f3-48c9-ae9f-728a1417b11a)



- Synthesizing the top level module
```
synth -top good_mux
```
![image](https://github.com/user-attachments/assets/3ec9b4a2-77b5-4a22-83e6-522b4960ee21)


- Mapping to the standard library
```
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
![image](https://github.com/user-attachments/assets/84da485a-6c61-43c0-8b3c-2a76183e8de7)



- To view the result as a graphich use the show command.
```
show
```
![image](https://github.com/user-attachments/assets/c270b01d-2195-4e2f-88fc-d9e8611c7e3f)






- To save the resulting netlist to a file, use the `write_verilog` command. This will generate the netlist and write it to a file in the current directory.
```
write_verilog -noattr good_mux_netlist.v
```

![image](https://github.com/user-attachments/assets/9e30bc4d-ed9a-45aa-bc85-283e20a9cbdf)


</details>

<details><summary><strong>Day2</strong></summary>

 ## Introduction to timing labs
 
- View the content in .lib file using the following commands.
	
```
cd ASIC/sky130RTLDesignAndSynthesisWorkshop/lib/

vim sky130_fd_sc_hd__tt_025C_1v80.lib
```

![image](https://github.com/user-attachments/assets/6eb59a5a-3244-4f38-a0ba-5c67a4ad4347)

![image](https://github.com/user-attachments/assets/4da889f4-bc72-4ed6-9709-ecf44d473522)


### Cell Library

A **standard cell library** is a collection of characterized logic gates designed for implementing digital circuits. The **Liberty (.lib)** files contain parameters related to Process, Voltage, and Temperature (PVT) that can significantly influence circuit performance. Variations in manufacturing processes, changes in voltage levels, and fluctuations in temperature all contribute to the overall functionality of the circuit.


![image](https://github.com/user-attachments/assets/55fab97d-4f59-4b93-9eb8-616454f65e2e)

![image](https://github.com/user-attachments/assets/56660781-4b59-4b87-85b3-1596d722c3a7)

![image](https://github.com/user-attachments/assets/b9a82d41-bd17-4fc5-9146-0c59fefa7610)

We can observe that:

    - and2_0 -- takes the least area, more delay and low power.
    - and2_1 -- takes more area, less delay and high power.
    - and2_2 -- takes the largest area, larger delay and highest power.

### Hierarchial synthesis vs Flat synthesis


**Hierarchical synthesis** involves decomposing a complex design into multiple sub-modules, each synthesized separately to create gate-level netlists before being integrated. This method improves organization, facilitates module reuse, and allows for incremental design changes without affecting the entire system.

In contrast, **flat synthesis** treats the entire design as a single entity during the synthesis process, resulting in one comprehensive netlist that disregards any hierarchical relationships. Although flat synthesis can optimize certain designs, it poses challenges in terms of maintenance, analysis, and modification due to the lack of structural modularity.


### Hierarchial synthesis : 

![image](https://github.com/user-attachments/assets/0a127ab8-8dc3-4f13-82d1-edc7914effa2)



- To perform hierarchical synthesis on the multiple_modules.v file use the following commands:

```c
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog multiple_modules.v


```

![image](https://github.com/user-attachments/assets/30b25818-aae1-4285-ab7c-68b54496d16c)


- Staistics of Multiple Modules

```c

synth -top multiple_modules
```

![image](https://github.com/user-attachments/assets/1910676b-c389-4c3b-8a5b-c3761100f761)

- Realization of the Logic

``` c

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show multiple_modules

```

![image](https://github.com/user-attachments/assets/e296f14e-0061-4150-9099-325945daef17)


- Map to the standard library
  
 ```c
 write_verilog -noattr multiple_modules_hier.v

!vim multiple_modules_hier.v

```

![image](https://github.com/user-attachments/assets/5657e44b-ee15-4a65-a0e0-419e83b41673)


### Flat synthesis :

- This process combines all hierarchical modules in the design into a single module to generate a flat netlist. To execute flat synthesis on the `multiple_modules.v` file, use the following commands


```c
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


![image](https://github.com/user-attachments/assets/d5cfea51-da55-4a69-af1c-39261408d042)

- Realization of the Logic

![image](https://github.com/user-attachments/assets/871e1c9a-fe52-4dd4-bdfd-dcad9c20d312)

- NetList File:

![image](https://github.com/user-attachments/assets/47cc71d1-1d27-4ddc-be48-5fc3f8d97b75)


### Sub Module Level Synthesis :

This approach is favored when multiple instances of the same module are utilized. The synthesis is performed once and then replicated multiple times, with the various instances of the module integrated into the top module. This method is particularly beneficial when employing a divide-and-conquer algorithm.

```c
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog multiple_modules.v

synth -top sub_module1

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show
```

![image](https://github.com/user-attachments/assets/f5b4e411-cb98-4f12-bf92-252174b6d445)


- Realization of the Logic

  ![image](https://github.com/user-attachments/assets/42a4253f-1460-486a-987b-cef4b4690cdc)


## Flop coding styles and optimization :

Flip-flops play a crucial role in sequential logic within a circuit, and in this section, we examine the design and synthesis of different types of flip-flops. To prevent glitches in digital circuits, flip-flops are employed to store intermediate values. This approach guarantees that the inputs to the combinational circuit remain stable until the clock edge, thereby avoiding glitches and ensuring correct operation.


### Asynchronous Reset/set:

#### Verilog Code for Asynchronous Reset:
![image](https://github.com/user-attachments/assets/966f1464-4b4e-477b-8f76-b3a98827db53)

#### Verilog Code Asynchronous Set:
![image](https://github.com/user-attachments/assets/52fc15f6-448c-409d-8d26-81182578ea9d)


- In this design, the `always` block is activated by changes in the clock or reset signal. The circuit is responsive to the positive edge of the clock. When the reset or set signal transitions to low or high, the output on the `q` line changes accordingly. As a result, the behavior linked to the reset or set occurs immediately, without waiting for the positive edge of the clock.




## Synchronous Reset:

![image](https://github.com/user-attachments/assets/99aba429-be7f-482d-bbfc-db6cbe31f67d)


## FLIPFLOP SIMULATION:

```c
iverilog dff_asyncres.v tb_dff_asyncres.v 

ls

./a.out

gtkwave tb_dff_asyncres.vcd
```

![image](https://github.com/user-attachments/assets/375ac486-d178-4d04-92fe-8e3340e65857)


#### gtk wave for ASYNCHRONOUS RESET:

![image](https://github.com/user-attachments/assets/d8c3489e-7c3a-48b9-9e9d-4201414eb7ca)


#### gtk wave for ASYNCHRONOUS SET:

![image](https://github.com/user-attachments/assets/dabaa6cd-1ce8-49b9-aa0e-b0403fc7164f)


#### gtk wave for SYNCHRONOUS RESET:

![image](https://github.com/user-attachments/assets/acc5c8fe-4b3f-407f-84a7-f270dc7db360)


## FLIP FLOP SYNTHESIS


#### Statistics of D FLipflop with Asynchronous Reset

```c
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog dff_asyncres.v

synth -top dff_asyncres

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

```

![image](https://github.com/user-attachments/assets/f1a04eb5-4b09-4392-81a3-4f69c9ca16db)

![image](https://github.com/user-attachments/assets/69653487-0af2-4e65-b6e1-9d58aebb85a0)


- Realization of Logic
![image](https://github.com/user-attachments/assets/c842b631-4e94-4dea-8d55-632b0def6561)

- We designed a flip-flop with an active high reset, but the flip-flop is actually functioning with an active low reset. Consequently, the tool inserted an inverter, resulting in `(!(!(reset)))`, which effectively simplifies to just `reset`. Ultimately, we end up with a flip-flop that has an active high reset.

#### Statistics of D FLipflop with Asynchronous set

```c
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog dff_async_set.v

synth -top dff_async_set

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

```
- Realization of Logic
![image](https://github.com/user-attachments/assets/6f0d5dee-023c-47c2-b23d-ee2b7b3949f4)

- We designed a flip-flop with an active high set, but the flip-flop is actually operating with an active low set. As a result, the tool inserted an inverter, leading to `(!(!(set)))`, which simplifies to just `set`. Consequently, we end up with a flip-flop that has an active high set.


#### Statistics of D FLipflop with synchronous reset

```c
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog dff_syncres.v

synth -top dff_syncres

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

```
- Realization of Logic
![image](https://github.com/user-attachments/assets/b7094cf6-d302-4c1a-a6b8-8df666c9417a)


## Optimizations :

### MULT2: 

![image](https://github.com/user-attachments/assets/a9c77e32-7c8e-48a5-a891-aef98b6899b0)


#### Truth Table :

![image](https://github.com/user-attachments/assets/59a9487c-b290-49ed-85f8-1f62f75be7e0)

We can observe that multiplying a number by 2 does not require additional hardware; it simply involves appending zeroes to the least significant bits (LSBs). The remaining bits remain the same as the input bits. This can be achieved by grounding the LSBs and correctly wiring the inputs to the outputs.


#### Statistics 

```c
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog mult_2.v

synth -top mul2

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show 

write_verilog -noattr mul_2_net.v

!vim mult_2_net.v
```

![image](https://github.com/user-attachments/assets/785317c2-eaa0-48be-a7d8-ac1ab1eb7a82)


-Realization of Logic :

![image](https://github.com/user-attachments/assets/e844a2c7-bd9f-45bb-9ec8-40cba38430c1)

-NetList:

![image](https://github.com/user-attachments/assets/c31101b6-a8ec-4b00-8e6d-930417b8fa9f)


### MULT8: 
![image](https://github.com/user-attachments/assets/082df5dd-9d09-43a9-91f3-ade33f7a9970)

#### LOGIC Bhevaiour:
![image](https://github.com/user-attachments/assets/9ee08f83-5cbf-43de-95a1-ae3c6ca2f05f)



#### Statistics 


```c
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

![image](https://github.com/user-attachments/assets/a25df668-a115-4963-87dc-31577cb13018)


-Realization of Logic :

![image](https://github.com/user-attachments/assets/7e7be7fe-e8c0-401d-9a7c-4dfd25703499)


-NetList:

![image](https://github.com/user-attachments/assets/e80ba410-45d0-41d4-9189-359ff0b449e7)

</details>


<details><summary><strong>Day3</strong></summary>
	
## Introduction to Combinational Logic Optimization and sequential Optimization

There are two types of optimizations: combinational and sequential optimizations. These optimizations are performed to create designs that are efficient in terms of area, power, and performance.

## Combinational Optimization

The techniques employed include:

- **Constant Propagation (Direct Optimization)**
- **Boolean Logic Optimization** (using K-Map or Quine-McCluskey method)

### Constant Propagation

Consider the circuit shown below:

![image](https://github.com/user-attachments/assets/4c60328f-06b4-4c2a-b746-77b92ff5be1a)

The top circuit utilizes 6 transistors (3 NMOS and 3 PMOS), whereas the bottom circuit uses only 2 transistors (1 NMOS and 1 PMOS) when input A is set to zero, effectively converting the logic into an inverter.

### Boolean Logic Optimization

Consider the following Verilog code:

```
assign y = a ? (b ? c : (c ? a : 0)) : (!c);
```

![image](https://github.com/user-attachments/assets/0de784cd-0cff-4783-bb88-4efc10245a36)

![image](https://github.com/user-attachments/assets/40b28eb2-71b5-40fb-917c-2079b0825ef3)

### Example 1:

#### Code:
![image](https://github.com/user-attachments/assets/7fce08f8-87b4-46c7-a6f0-efc8bda0c46d)

#### Simulation:


```c
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

![image](https://github.com/user-attachments/assets/152cd679-a189-401d-8cf4-2966624a5a6f)
![image](https://github.com/user-attachments/assets/fcf7fdf6-319b-43a8-bdf5-a2a9bf2bcb3a)


### Example 2:

##### Code:
![image](https://github.com/user-attachments/assets/c3200db0-ab77-4b85-af9f-a16436aa65cf)

#### Simulation:
```
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

![image](https://github.com/user-attachments/assets/14e12c28-2bdb-420e-9df5-89e577b42e4f)

![image](https://github.com/user-attachments/assets/8074197d-d3c2-4a17-9513-039f9c702040)




### Example 3:
#### Code:
![image](https://github.com/user-attachments/assets/5e7245bd-de0d-4e8c-9e78-1c74fafdb3eb)

#### Simulation:
```
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

![image](https://github.com/user-attachments/assets/83027638-563a-4bef-85b4-dd0d1ac1f2b7)

![image](https://github.com/user-attachments/assets/a0d62911-fc84-43b4-b294-5c79eeb7cd09)



### Example 4:
#### Code:
![image](https://github.com/user-attachments/assets/96ded852-8249-455b-89b0-74beb4415051)

#### Simulation:
```c
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog opt_check4.v

synth -top opt_check4

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr opt_check4_net.v

!vim opt_check4_net.v
````

![image](https://github.com/user-attachments/assets/0cafde7c-7649-48cf-aa1f-e0f3108623f3)

![image](https://github.com/user-attachments/assets/708323e6-8713-4f87-a116-f1463d096a58)


### Example 5:
#### Code:
![image](https://github.com/user-attachments/assets/a6c9da23-50a0-400b-8c4f-904ec988523e)

#### Synthesis

```c
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
````

![image](https://github.com/user-attachments/assets/774b8041-82cb-4477-8c37-44f305696f6b)

![image](https://github.com/user-attachments/assets/243a7600-f20f-4d25-9289-b0312bcf0174)


### Example 6
#### Code:
![image](https://github.com/user-attachments/assets/1d8779ea-bea6-4ecd-9c1c-46f269a427f4)

#### Synthesis:

```c
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

![image](https://github.com/user-attachments/assets/5a3c2c2e-83be-4556-8d05-c31defae62c0)

![image](https://github.com/user-attachments/assets/4564662d-de7f-45d2-89b4-b906fb33097d)



## Sequential Logic Optimizations:

### Example 1:
#### Code:
![image](https://github.com/user-attachments/assets/375695d7-9a3d-48a6-b74b-f8cb5979d4c6)


#### Simualtion:

```c
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
![image](https://github.com/user-attachments/assets/056f0ee9-494e-4e74-9c89-12757de9308c)

![image](https://github.com/user-attachments/assets/2e56cd2e-eefc-4144-bcbf-4c306c50a0e1)

#### GTK Wave Output:

```c
iverilog dff_const1.v tb_dff_const1.v

./a.out

gtkwave tb_dff_const1.vcd
```
![image](https://github.com/user-attachments/assets/d289e977-1dfa-4fee-8b79-ad7d06d93fd6)


### Example 2:
#### Code:
![image](https://github.com/user-attachments/assets/c4d39163-9f53-42f0-9c02-7720eb3dbf22)


#### Simualtion:

```c
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

![image](https://github.com/user-attachments/assets/1fe738ec-1eee-469f-87d7-354154490195)


![image](https://github.com/user-attachments/assets/21008055-d542-46bd-9ccf-8bc4fe2a461f)

#### GTK Wave Output:

```c
iverilog dff_const2.v tb_dff_const2.v

./a.out

gtkwave tb_dff_const2.vcd
```


![image](https://github.com/user-attachments/assets/4b0f8491-2878-4007-853f-08371c7d1c5b)



### Example 3:
#### Code:

![image](https://github.com/user-attachments/assets/f31ee24f-00ee-453b-bb52-c6bf58bd15c6)


#### Simuations:

```c
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog dff_const3.v

synth -top dff_const3

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr dff_const3_net.v
```


![image](https://github.com/user-attachments/assets/9405d785-a9e0-4151-a216-fb6bfbe7bb68)


![image](https://github.com/user-attachments/assets/3a0717d8-b3fe-47a3-98ac-b3480bb5aa5e)


#### GTK Wave Output:

```c
iverilog dff_const3.v tb_dff_const3.v

./a.out

gtkwave tb_dff_const3.vcd
```

![image](https://github.com/user-attachments/assets/b325ccb2-a5b5-4fd3-95ff-573748d12647)



### Example 4:
#### Code:

![image](https://github.com/user-attachments/assets/8f6bdea4-95d5-4fd2-adfd-cf911866f506)



#### Simuations:

```c
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog dff_const4.v

synth -top dff_const4

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr dff_const4_net.v
```

![image](https://github.com/user-attachments/assets/54925845-d0b4-4536-ba1d-992a7b87690d)

![image](https://github.com/user-attachments/assets/51158ab4-a4eb-4311-bc40-179e998b110d)



#### GTK Wave Output:

```c
iverilog dff_const4.v tb_dff_const4.v

./a.out

gtkwave tb_dff_const4.vcd
```

![image](https://github.com/user-attachments/assets/63241039-025d-4da2-bdb2-e6ac83be8e69)



### Example 5:
#### Code:

![image](https://github.com/user-attachments/assets/640c9cdb-e142-4681-803e-56e0ad4db17a)




#### Simuations:

```c
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog dff_const5.v

synth -top dff_const5

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr dff_const5_net.v
```
![image](https://github.com/user-attachments/assets/077e4299-0401-4480-95bf-d0779dfd4b08)

![image](https://github.com/user-attachments/assets/9b65bdb8-5bf5-48d2-a7cb-32ce5e2d6f64)




#### GTK Wave Output:

```c
iverilog dff_const5.v tb_dff_const5.v

./a.out

gtkwave tb_dff_const5.vcd
```
![image](https://github.com/user-attachments/assets/15993fa4-e716-48b7-90ce-a1fb124feae3)



## Sequential Logic Optimizations for unused outputs:

### Example 1:
#### Code:
![image](https://github.com/user-attachments/assets/78766646-dc9f-420e-951f-b22a2e4fe74b)

#### Synthesis:

```c
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog counter_opt.v

synth -top counter_opt

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr counter_opt_net.v
```

![image](https://github.com/user-attachments/assets/b7d9cce5-442a-4ff0-889d-6ffd2880b031)

![image](https://github.com/user-attachments/assets/92136998-c7bc-4249-9fd1-16135f3aaf93)


#### GTK Wave Output:

```c
iverilog counter_opt.v tb_counter_opt.v

./a.out

gtkwave tb_counter_opt.vcd
```

![image](https://github.com/user-attachments/assets/80cbc3be-fd16-4b30-befe-6f7c76f16bee)



### Example 2:
#### Code:
![image](https://github.com/user-attachments/assets/5757e4bc-dcec-482d-a017-41f8b613d0a6)


#### Synthesis:

```c
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog counter_opt2.v

synth -top counter_opt2

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr counter_opt_net.v
```

![image](https://github.com/user-attachments/assets/43160d4a-97b7-4aa2-a5ac-59b627f47818)


![image](https://github.com/user-attachments/assets/9a77efb7-abd0-4873-8e7b-1cdeeefa785c)



#### GTK Wave Output:

```c
iverilog counter_opt2.v tb_counter_opt.v

./a.out

gtkwave tb_counter_opt.vcd
```

![image](https://github.com/user-attachments/assets/f8ab9668-8699-456c-86b9-dcbdbdb4bcf2)



</details>

<details><summary><strong>Day4</strong></summary>
	
## Introduction to GLS and Synthesis-Simulation mismatch
### Gate Level Simulation (GLS)
- **Purpose:** Verifies the synthesized netlist, ensuring logical accuracy and proper timing behavior.
- **Process:** 
  - Uses a testbench to simulate the netlist, a lower-level design representation.
  - Compares simulation outputs with expected results to confirm correctness.
- **Goal:** Ensure that the synthesis process hasn't introduced errors and that the design meets performance requirements.

### Key Considerations:
1. **Sensitivity Lists:**
   - Critical for ensuring correct circuit behavior.
   - An incomplete sensitivity list can lead to unintended latches.
   
2. **Blocking and Non-blocking Assignments:**
   - **Blocking assignments** (`=`) and **non-blocking assignments** (`<=`) behave differently in `always` blocks.
   - Improper use of blocking assignments can unintentionally create latches, causing mismatches between synthesis and simulation.

### Conclusion:
- To avoid issues, carefully review circuit behavior.
- Ensure the sensitivity list and assignments align with the intended functionality.

![image](https://github.com/user-attachments/assets/959fe922-9ca4-4392-b3d6-ab5b9efd4372)


### Example 1

#### Code:
![image](https://github.com/user-attachments/assets/7b48beef-e3fe-42ea-a2c1-80860f8b6091)


#### Simulation:

```c
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog ternary_operator_mux.v

synth -top ternary_operator_mux

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr ternary_operator_mux_net.v


```

![image](https://github.com/user-attachments/assets/ecb7601a-8622-459c-a8d9-118f896c836e)

![image](https://github.com/user-attachments/assets/acbb2f20-270c-4d60-a839-ac8b1786a8b5)


#### GTK Wave:

```c
iverilog ternary_operator_mux.v tb_ternary_operator_mux.v

./a.out

gtkwave tb_ternary_operator_mux.vcd
```

![image](https://github.com/user-attachments/assets/6156df8b-f5b3-41a3-87ca-a1d018038a0b)


#### GLS:

```C
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v

./a.out

gtkwave tb_ternary_operator_mux.vcd
```


![image](https://github.com/user-attachments/assets/e7213e5a-80a8-44d2-8329-a49fe2fb3821)


### Example 2

#### Code:
![image](https://github.com/user-attachments/assets/436c2089-88f7-47f4-87a1-d7b43a3bdb09)



#### Synthesis:

```c
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog bad_mux.v

synth -top bad_mux

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr bad_mux_net.v



```
![image](https://github.com/user-attachments/assets/4c943a0d-3d98-449d-aa44-c9aa53ef06e2)

![image](https://github.com/user-attachments/assets/321938db-5fb1-4e3f-8282-ca6348710f01)



#### GTK Wave:

```c
iverilog bad_mux.v tb_bad_mux.v

./a.out

gtkwave tb_bad_mux.vcd
```

![image](https://github.com/user-attachments/assets/5f437f5a-a258-410f-9e8a-4b7fc14be6ca)



#### GLS:

```C
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_mux_net.v tb_bad_mux.v

./a.out

gtkwave tb_bad_mux.vcd
```

### Labs on Synthesis-Simulation mismatch for blocking statements :

#### Code:
![image](https://github.com/user-attachments/assets/8839e498-5314-4563-ac96-6843206c65bd)


#### Synthesis:

```c
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog blocking_caveat.v

synth -top blocking_caveat

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

write_verilog -noattr blocking_caveat_net.v
```

![image](https://github.com/user-attachments/assets/4c52b740-132c-493f-b027-5e1a71e854db)

![image](https://github.com/user-attachments/assets/c847c033-7c12-473c-9866-b5b8b8caa994)


#### GTK Wave:

```c

iverilog blocking_caveat.v tb_blocking_caveat.v

./a.out

gtkwave tb_blocking_caveat.vcd

```

![image](https://github.com/user-attachments/assets/47347c00-bbfd-49e2-a56a-b4e25e8bba40)



#### GLS:

```c
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v blocking_caveat_net.v tb_blocking_caveat.v

./a.out

gtkwave tb_blocking_caveat.vcd
```


![image](https://github.com/user-attachments/assets/7e1b25e0-239d-4f85-b426-9d12c91b8672)



In this case there is a synthesis and simulation mismatch. While performing synthesis yosys has corrected the latch error.


</details>
</details>

<details><summary><strong>Lab10</strong></summary>


## Synthesizing RISC-V and comparing output with functional (RTL) simulation.
### Procedure
- Go to Following Directory
```c
  cd /home/nikhil/ASIC/sky130RTLDesignAndSynthesisWorkshop/src/module
```
- Run the following commands
```c
yosys
read_liberty -lib /home/nikhil/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
![image](https://github.com/user-attachments/assets/de63c797-df7b-47e8-baa5-b0a477154578)

```
read_verilog clk_gate.v
```
![image](https://github.com/user-attachments/assets/586d6f43-43aa-4469-9ad6-d097ea283196)


```
read_verilog rvmyth.v
```
![image](https://github.com/user-attachments/assets/a465e7ad-97cb-4de0-992e-d0f5323b729e)


```
synth -top rvmyth
```
![image](https://github.com/user-attachments/assets/2f4428b7-e8aa-4100-add5-6e6777758776)


```
abc -liberty /home/nikhil/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
![image](https://github.com/user-attachments/assets/dfc95b7b-427e-4d36-b301-eb1740424504)


```
write_verilog -noattr rvmyth_netlist.v
exit
```

![image](https://github.com/user-attachments/assets/42e4815c-c5ae-44ad-83b5-3fe70e4aa1e0)

![image](https://github.com/user-attachments/assets/761f4322-2c81-4c84-95ec-1a650cac4888)






```
iverilog /home/nikhil/VLSI/sky130RTLDesignAndSynthesisWorkshop/my_lib/verilog_model/primitives.v /home/nikhil/VLSI/sky130RTLDesignAndSynthesisWorkshop/my_lib/verilog_model/sky130_fd_sc_hd.v rvmyth.v testbench.v vsdbabysoc.v avsddac.v avsdpll.v clk_gate.v
./a.out
gtkwave dump.vcd
```

![image](https://github.com/user-attachments/assets/7d682764-8a39-4e5d-81c4-27c430c42f1a)


![image](https://github.com/user-attachments/assets/576403fd-06bc-4c07-b6c7-348bd18eb45b)

![image](https://github.com/user-attachments/assets/f7026f6e-dbd1-4be2-b77d-9bac2c6dec6e)




## Realization :

![image](https://github.com/user-attachments/assets/3bdabc2d-42b6-4522-987b-7fb8cee067ce)


![image](https://github.com/user-attachments/assets/a5b931a5-353a-43d5-860b-1af75e52b21d)





## RTL Simulations:

- Run the following commands to view the RTL Simulation of BabySoc.
```c
cd BabySoC_Simulation/

iverilog -o ./pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module/

./pre_synth_sim.out

gtkwave pre_synth_sim.vcd
```
- You can view 20 cycles in the below image
![image](https://github.com/user-attachments/assets/4dc3d369-b8d3-4abd-9a32-5746f87a6ee0)


- You can view numbers adding up from 1 to 9 
![image](https://github.com/user-attachments/assets/c20071ea-2444-4a5b-925f-2504ac3b690a)

</details>

<details><summary><strong>Lab11</strong></summary>
	
# Static Timing Analysis (STA)

- **Definition**:  
  Static Timing Analysis (STA) is a technique to verify the timing performance of digital circuits without dynamic simulation. It ensures that the circuit meets timing constraints by analyzing data paths, gate delays, and interconnects.

- **Key Functions**:
  - **Setup & Hold Time Verification**:  
    Checks for violations to ensure data stability around clock edges.
  - **Clock Properties**:  
    Incorporates clock parameters like frequency, skew, and jitter.
  - **Worst-Case Delay Conditions**:  
    Assumes worst-case delays to verify performance across varying conditions.

- **Tools**:  
  - Common STA tools include **Synopsys PrimeTime** and **Cadence Tempus**.

- **Importance in Digital Design**:
  - **Data Propagation Verification**:  
    Ensures data signals propagate within required timing limits for valid outputs.
  - **Violation Identification**:  
    Detects setup and hold violations to ensure reliable operation of flip-flops and other sequential elements.
  - **Performance Optimization**:  
    Highlights critical paths that restrict maximum operating frequency.
  - **Early Detection of Timing Issues**:  
    Minimizes costly redesigns by detecting issues in early stages.
  - **Power-Performance Balance**:  
    Analyzes power impact of clock frequency adjustments.
  - **Handling Complex Designs**:  
    Considers variations in manufacturing, temperature, and voltage for consistent performance.

---

## Reg-to-Reg Path

- **Definition**:  
  A **reg-to-reg path** connects two sequential elements (like flip-flops) within a circuit.

- **Purpose**:  
  Ensures data flows correctly between registers in pipelined or sequential designs.

- **Key STA Considerations**:
  - **Setup & Hold Timing Compliance**:  
    Ensures data stability at registers.
  - **Delay Analysis**:  
    Assesses delays through combinational logic between registers.
  - **Critical Path Impact**:  
    Determines maximum circuit frequency.
  - **Multi-Clock Domain Considerations**:  
    Accounts for metastability and synchronization when registers are in different clock domains.

---

## Clk-to-Reg Path

- **Definition**:  
  A **clk-to-reg path** connects the clock signal to a register, ensuring correct register operation relative to clock edges.

- **Importance in STA**:
  - **Clock Delay Analysis**:  
    Measures delay for the clock signal from the source to
## STA for Synthesized RISC-V Core with 10.70 ns Time Period

To verify that the synthesized RISC-V Core module meets its timing constraints, we will generate setup and hold timing reports. These reports confirm that data signals propagate correctly throughout the core.

### Steps to Run Timing Analysis

Run the following commands to generate setup and hold timing reports:

```
set PERIOD 10.7

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
- Now run these commands
```
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

![image](https://github.com/user-attachments/assets/da67530f-61e1-4510-ae4c-806842dc8509)


**Set Up Time**

![image(4)](https://github.com/user-attachments/assets/565ad794-cc12-404e-b85e-4861981ad946)


**Hold Time**

![image(5)](https://github.com/user-attachments/assets/6f00909e-2db1-4292-96b8-abff2de760b8)



</details>

<details><summary><strong>Lab12</strong></summary>

## STA comparison for various sky libraries :

- Store all the sky lib files in a folder named timing_libs. Now, go to VSDBabySoC/src and create a tickle file sta_across_pvt.tcl . The below block is the content of the tickle file

```c
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


![image(7)](https://github.com/user-attachments/assets/6a9e2606-d44e-4019-9ac2-1955b128840b)

constrains file :

![image](https://github.com/user-attachments/assets/f17a35df-dff5-4983-9981-d57ab8698dc7)



Run the following commands:

```c
cd VSDBabySoC/src

sta

source sta_across_pvt.tcl
```


### Table:
![image](https://github.com/user-attachments/assets/4d17bcb5-e5c0-4425-a3c0-f288905fa0bc)


### Graphs:


#### Worst Setup Slack:
![image](https://github.com/user-attachments/assets/49818caa-6464-48b3-9ea0-df5f41a8af03)

#### Worst Hold Slack:
![image](https://github.com/user-attachments/assets/7092d8b3-a272-4511-a836-d09a1bf1bb4c)




### Timing Reports:
![image](https://github.com/user-attachments/assets/c66c870b-d705-4c74-ba60-83908af4b768)
![image](https://github.com/user-attachments/assets/cb4feafb-b399-4d85-ba1d-b4661955bbba)
![image](https://github.com/user-attachments/assets/ccf3d87a-1dce-4a6b-a2b2-d59c1d12e330)
![image](https://github.com/user-attachments/assets/ff42f908-70db-40fd-9367-113f6c34c403)
![image](https://github.com/user-attachments/assets/1b2c6d81-ea21-4376-afc9-814a43471279)
![image](https://github.com/user-attachments/assets/fad27c9a-3fe8-471f-8678-68e190e53639)
![image](https://github.com/user-attachments/assets/fbeecc3b-2c13-4826-881d-512d5686ac93)
![image](https://github.com/user-attachments/assets/eeccebfb-b094-4ff1-b3a9-cf4c1e75c6ca)
![image](https://github.com/user-attachments/assets/5f627eb6-2a19-48f9-87e1-eee3256625a3)
![image](https://github.com/user-attachments/assets/1ff80db5-9bf4-4711-81fc-d73bf5cf8811)
![image](https://github.com/user-attachments/assets/98223880-f075-4033-8fa4-3998c87b3aa5)
![image](https://github.com/user-attachments/assets/4608789b-1475-4e3c-b2c0-bf7bc845c202)
![image](https://github.com/user-attachments/assets/5294c0f2-194c-4e42-afab-9298c9c4fa08)
![image](https://github.com/user-attachments/assets/7e3850fa-76b9-49ce-964c-9f8cec1a9f98)
![image](https://github.com/user-attachments/assets/65a0443c-9d27-49f3-a361-f7e6bd4c7ed2)
![image](https://github.com/user-attachments/assets/377d470e-e213-42d5-8fc7-88e9e200a29a)
![image](https://github.com/user-attachments/assets/96be1477-09d7-4f9c-af33-eacda8c909bc)
![image](https://github.com/user-attachments/assets/d6da0026-e82b-4e83-80db-8631dac54cdf)
![image](https://github.com/user-attachments/assets/748dedc5-988b-4764-b20d-a69f0d2917dc)
![image](https://github.com/user-attachments/assets/78ba15fc-0bb8-44c7-9d3d-92ca22e6a474)
![image](https://github.com/user-attachments/assets/c929b122-76b3-4a30-824d-5bc2ba6060d6)
![image](https://github.com/user-attachments/assets/e4c5acae-d4e0-48ef-9d1e-8333bd799228)





</details>

<details>
<summary><strong>Lab13</strong></summary>
<details>
	
# Advanced Physical Design using OpenLane using Sky130
<summary><strong>Day-1:</strong>  Inception of open-source EDA, OpenLane and Sky130 PDK</summary>

## Origin of Open-Source EDA, OpenLane, and Sky130 PDK

**QFN-48 Package**: The QFN-48 is a compact, leadless package with 48 connection pads on its perimeter, known for its efficient thermal and electrical properties. It’s well-suited for applications requiring high component density.

![Screenshot from 2024-11-11 14-32-57](https://github.com/user-attachments/assets/3f1237c7-c6a6-453f-9655-636f8778eb9c)

**Chip**: This is an integrated circuit (IC) built on a silicon substrate, consisting of various functional blocks, such as memory, processors, and I/O, each designed for specific electronic applications.

![Screenshot from 2024-11-11 14-33-11](https://github.com/user-attachments/assets/32a46c31-bde4-46b7-807a-5f09e7a42a82)

**Pads**: Small metallic contact points on a chip or package, allowing internal circuitry to connect with external components and enabling signal transfer.

**Core**: The main processing unit within a chip, containing logic designed for optimized power and performance.

**Die**: This is the individual IC section of a silicon wafer before packaging, containing all active chip circuitry.

![Screenshot from 2024-11-11 14-33-23](https://github.com/user-attachments/assets/ff797bc6-ea8e-4bd9-be22-e3cf8ce4df95)

**IPs (Intellectual Properties)**: These are pre-designed functional modules, such as USB controllers or memory interfaces, licensed for reuse to reduce development time and costs.

![Screenshot from 2024-11-11 14-33-32](https://github.com/user-attachments/assets/4e0b10e5-eea2-4b68-a42c-3db071423e38)

---

### Software-to-Hardware Execution Flow

When running an application on hardware, it starts at the system software layer, translating it to binary form. Key components here include the Operating System (OS), Compiler, and Assembler.

1. **OS**: Breaks down application functions written in high-level languages (e.g., C, Java) and passes them to a compiler.
2. **Compiler**: Translates functions into hardware-specific low-level instructions.
3. **Assembler**: Converts these instructions into binary code executable by the hardware.

![Screenshot from 2024-11-11 14-33-50](https://github.com/user-attachments/assets/0f07a5f9-9ce0-457e-a7af-1caffa64f809)

For example, a stopwatch application running on a RISC-V core follows this flow: The OS generates a function in C, which the compiler translates into RISC-V instructions adapted to the architecture. These instructions are further processed by the assembler, converting them into binary code, which is integrated into the chip layout to allow hardware execution of the intended function.

![Screenshot from 2024-11-11 14-34-04](https://github.com/user-attachments/assets/1d9b1e0e-1170-4709-98b9-3f2f71f28098)

Below is an example showing the compiler’s input and output for the stopwatch app.

![Screenshot from 2024-11-11 14-34-16](https://github.com/user-attachments/assets/205f26fb-62b5-40d4-8bf2-ef96fc9ff711)

The compiler generates instructions specific to the architecture, and the assembler converts these into binary patterns. To execute them, an RTL (written in a Hardware Description Language) interprets and implements these instructions. This RTL design is then synthesized into a netlist, represented by interconnected logic gates. Finally, the netlist undergoes physical design implementation, preparing it for chip fabrication.

![Screenshot from 2024-11-11 14-34-28](https://github.com/user-attachments/assets/f4df4c33-62e9-4a78-9f14-98282aa9c2a2)

---

### Components of ASIC Design

- **RTL IPs**: Verified circuit blocks (adders, flip-flops) in HDL used to speed up complex designs.
- **EDA Tools**: Automate ASIC tasks like synthesis, optimization, and timing analysis, ensuring that performance requirements are met.
- **PDK Data**: Foundry files define the manufacturing process, ensuring that ASIC designs are fabrication-ready.

![Screenshot from 2024-11-11 14-34-40](https://github.com/user-attachments/assets/610fc21f-8262-4d4b-b868-01a9d84b40ca)

---

### Simplified RTL to GDSII Flow

![Screenshot from 2024-11-11 14-34-49](https://github.com/user-attachments/assets/9d8d2664-476d-4cd6-b5b3-7e0aed241a2f)

1. **RTL Design**: Describes the circuit’s function using HDLs like Verilog or VHDL.
2. **RTL Synthesis**: Converts RTL to a gate-level netlist with optimized cells.
3. **Floor and Power Planning**: Arranges major components, power grid, and I/O.
4. **Placement**: Allocates cells to minimize wirelength and signal delay.
5. **Clock Tree Synthesis (CTS)**: Distributes clock signals evenly to reduce skew.
6. **Routing**: Connects components while adhering to design rules.
7. **Sign-off**: Final verification, confirming design readiness for fabrication.
8. **GDSII Generation**: Creates the layout for chip production.

---

### OpenLane ASIC Flow Overview

![Screenshot from 2024-11-11 14-34-58](https://github.com/user-attachments/assets/414fbce3-7665-4375-9123-42a11f41ccec)

1. **RTL Synthesis and Technology Mapping**: Utilizes Yosys and ABC.
2. **Static Timing Analysis**: Performed with OpenSTA.
3. **Floor Planning**: Uses init_fp, ioPlacer, pdn, and tapcell.
4. **Placement**: Managed by RePLace, Resizer, OpenPhySyn, and OpenDP.
5. **Clock Tree Synthesis**: Executed by TritonCTS.
6. **Fill Insertion**: Performed by OpenDP.
7. **Routing**: Uses FastRoute/CU-GR for global and TritonRoute/DR-CU for detailed routing.
8. **SPEF Extraction**: OpenRCX is used for parasitic data.
9. **GDSII Output**: Produced with Magic and KLayout.
10. **Design Rule Checks**: Conducted with Magic and KLayout.
11. **Layout vs. Schematic Check**: Uses Netgen.
12. **Antenna Checks**: Managed by Magic.

---

### OpenLane Directory Structure

```plaintext
├── OpenLane            # Tool directory
│   ├── designs         # Holds all designs
│   │   └── picorv32a   # Example design
├── pdks                # PDK-related files
│   ├── skywater-pdk    # Skywater 130nm PDKs
│   ├── open-pdks       # Scripts for open-source tool compatibility
│   ├── sky130A         # Open-source compatible PDK variant
│   │   ├── libs.ref    # Node-specific files (e.g., timing, tech LEF)
│   │   ├── libs.tech   # Tool-specific files (e.g., for KLayout)
```

### Running Synthesis in OpenLane

1. **Navigate and start OpenLane**:
   ```bash
   cd Desktop/work/tools/openlane_working_dir/openlane
   docker
   ./flow.tcl -interactive
   package require openlane 0.9
   prep -design picorv32a
   run_synthesis
   ```
   
   ![image](https://github.com/user-attachments/assets/582efdd1-9a5e-4cba-8e21-7ae875666f08)

2. **View the Netlist**:
   ```bash
   cd designs/picorv32a/runs/13-11_08-05/results/synthesis/
   gedit picorv32a.synthesis.v
   ```
![image](https://github.com/user-attachments/assets/bf2e3ab9-536f-4d78-a224-c3b62eb2ef96)

![image](https://github.com/user-attachments/assets/827d9144-39f8-4b34-a3b9-a7ad8d953954)


3. **Yosys Report**:
   ```bash
   cd ../..
   cd reports/synthesis
   gedit 1-yosys_4.stat.rpt
   ```

![image](https://github.com/user-attachments/assets/b015667f-4180-4e0e-88fb-57158c7dcbba)



 ![image](https://github.com/user-attachments/assets/41f85e0e-1a2a-46d5-bc2f-620377db27fa)

**Report:**

```
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

**Report Summary**:

```
Flop ratio = Number of D Flip flops = 1613  = 0.1084
             ______________________   _____
             Total Number of cells    14876
```

- **Wire Count**: 14,596
- **Cell Count**: 14,876, including specific cells like `sky130_fd_sc_hd__a2111o_2`, `sky130_fd_sc_hd__and2_2`.
- **D Flip-flops**: 1,613 with a flop ratio of 0.1084

</details>

<details>
<summary><strong>Day-2:</strong>  Good floorplan vs bad floorplan and introduction to library cells </summary>

## Good floorplan vs bad floorplan and introduction to library cells 
 
### **Utilization Factor and Aspect Ratio**
In IC floor planning, the utilization factor and aspect ratio are key parameters. The utilization factor is defined as the area occupied by the netlist divided by the total core area. Although a utilization factor of 1 (100%) is ideal, practical designs aim for a factor of 0.5 to 0.6 to provide room for buffer zones, routing channels, and adjustments. The aspect ratio, calculated as height divided by width, determines the shape of the chip. An aspect ratio of 1 indicates a square, while other values create a rectangular layout, with the specific ratio chosen based on functional, packaging, and manufacturing requirements.

```
Utilization Factor =  Area occupied by netlist
                     __________________________
                         Total area of core
                         

Aspect Ratio =  Height
               ________
                Width
```

### **Pre-placed Cells**
Pre-placed cells are key functional blocks—like memory, custom processors, and analog circuits—that are placed manually in fixed positions. These blocks are crucial for performance and are kept fixed during placement and routing to maintain functionality and layout integrity.

### **Decoupling Capacitors**
Decoupling capacitors stabilize power supply voltages near logic circuits during transient events. Acting as local energy reserves, they reduce voltage fluctuations, crosstalk, and EMI, ensuring reliable power delivery to sensitive circuits.

### **Power Planning**
Effective power planning involves creating a power and ground mesh to evenly distribute VDD and VSS across the chip. This arrangement stabilizes power delivery, reduces voltage drops, and enhances efficiency. Multiple power and ground points further support power needs and reduce the risks of instability.

### **Pin Placement**
Strategic pin placement, or I/O planning, is essential for functionality and reliability. Proper pin assignments minimize signal degradation, manage heat dissipation, and support thermal stability. Placing power and ground pins strategically enhances both signal strength and manufacturability.

### **Floorplanning with OpenLANE**
To initiate floorplanning in OpenLANE, execute these commands:

```
cd Desktop/work/tools/openlane_working_dir/openlane
docker
```

```
run
package require openlane 0.9
prep -design picorv32a
run_synthesis
run_floorplan
```

![image](https://github.com/user-attachments/assets/f463e550-f17f-4fc6-a8ff-447cd0987a05)


![image](https://github.com/user-attachments/assets/a65d135a-f4a3-4afc-99a9-e86f1d257317)


Then, in a new terminal, access the floorplan file as follows:

```
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_08-38/results/floorplan
gedit picorv32a.floorplan
```
![image](https://github.com/user-attachments/assets/33150b0b-e440-41e5-820c-ee8488015046)


According to the floorplan definitions:

- 1000 Unit Distance = 1 Micron
- Die width in unit distance = 660685−0 = 660685  
- Die height in unit distance = 671405−0 = 671405  
- Width in microns = 660685 / 1000 = 660.685 Microns  
- Height in microns = 671405 / 1000 = 671.405 Microns  
- Die area in microns² = 660.685 × 671.405 = 443587.212425 Microns²  

To view the floorplan in Magic:

```
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_08-56/results/floorplan/
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```
![image](https://github.com/user-attachments/assets/f354eeef-17c7-4b14-9eda-1faccdfdcc1f)



### **Decap and Tap Cells**

Decap cells and tap cells are placed to manage power delivery and maintain electrical connectivity across the chip.

![image](https://github.com/user-attachments/assets/46f8340d-cb72-4a8d-b13c-caca3b92145e)



### **Unplaced Standard Cells at Origin**

![image](https://github.com/user-attachments/assets/9c8fbc27-141e-48ff-8e42-740fb62e76e6)


```
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
run_synthesis
run_floorplan
run_placement
```

![image](https://github.com/user-attachments/assets/baf4d5af-f3a9-4cb6-98cb-19887811bb43)


View the placement in Magic with:

```
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_08-56/results/placement/
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

![image](https://github.com/user-attachments/assets/68ea7089-2e64-4c37-b281-9d799d8ab364)


![image](https://github.com/user-attachments/assets/4bdb2653-05a2-45f8-a76b-75f99a0067b0)


### **Cell Design and Characterization Flow**
The library provides essential cell information, including various sizes, functionalities, and threshold voltages. A typical design flow includes:

- Inputs: Process Design Kits (PDKs) for DRC & LVS, SPICE models, and library/user-defined specs.
- Steps: Circuit design, layout design (using techniques like Euler’s path and stick diagrams), parasitic extraction, and characterization (timing, noise, power).
- Outputs: CDL, LEF, GDSII, extracted SPICE netlist (.cir), and timing/noise/power .lib files.

### **Standard Cell Characterization Flow**
Standard cell characterization includes:

1. Reading models and tech files
2. Loading the extracted SPICE netlist
3. Recognizing cell behavior
4. Reading subcircuits
5. Applying power sources
6. Stimulating characterization setup
7. Setting output capacitance loads
8. Running simulation commands

These steps are processed by the GUNA software to produce timing, noise, and power models.

### **Timing Parameters**

| Timing Definition     | Value       |
|-----------------------|-------------|
| slew_low_rise_thr     | 20%         |
| slew_high_rise_thr    | 80%         |
| slew_low_fall_thr     | 20%         |
| slew_high_fall_thr    | 80%         |
| in_rise_thr           | 50%         |
| in_fall_thr           | 50%         |
| out_rise_thr          | 50%         |
| out_fall_thr          | 50%         |

**Propagation Delay**: Time for an input change to reach 50% of its final value and affect the output similarly.

```
rise delay =  time(out_fall_thr) - time(in_rise_thr)
```

**Transition Time**: Time taken for signal transitions between 10% to 90% or 20% to 80% of signal levels.

```
Fall transition time: time(slew_high_fall_thr) - time(slew_low_fall_thr)
Rise transition time: time(slew_high_rise_thr) - time(slew_low_rise_thr)
```

</details>
<details>
<summary><strong>Day-3:</strong> Design library cell using Magic Layout and ngspice characterization </summary>

## CMOS Inverter NGSPICE Simulations

#### SPICE Deck for a CMOS Inverter Simulation

1. **Netlist Creation**: Define a netlist for the CMOS inverter circuit, labeling nodes clearly (e.g., `input`, `output`, `ground`, and `supply`) for easier identification during SPICE simulation.

2. **Device Sizing**: Define the Width-to-Length (W/L) ratios for the PMOS and NMOS transistors. Typically, the PMOS width is 2x to 3x the NMOS width for balanced drive strength.

3. **Voltage Levels**: Set gate and supply voltages in multiples of the transistor length.

4. **Node Naming**: Assign names (e.g., `VDD`, `GND`, `IN`, `OUT`) to nodes around each component in the SPICE netlist for easier reference.

   <img width="953" alt="Screenshot 2024-11-12 at 11 23 10 PM" src="https://github.com/user-attachments/assets/cbd5664c-8342-4f0c-9cc9-1320c3d1ca68">


5. **Simulation Commands**: For transient analysis, use:
   ``` 
   source [filename].cir 
   run 
   setplot 
   dc1 
   plot out vs in 
   ```


   <img width="938" alt="Screenshot 2024-11-12 at 11 23 32 PM" src="https://github.com/user-attachments/assets/4339530d-407c-4be8-a2de-ecdac84d3523">

#### Simulation and Analysis

- **Switching Threshold (Vm)**: Vm is the input voltage at which the inverter output switches between logic levels. For equal PMOS/NMOS sizes, Vm is around VDD/2. Adjusting PMOS or NMOS sizes shifts Vm higher or lower.

- **SPICE Command for Threshold Calculation**:
   ``` 
   Vin in 0 2.5 
   .op 
   .dc Vin 0 2.5 0.05 
   ```

    <img width="950" alt="Screenshot 2024-11-12 at 11 23 49 PM" src="https://github.com/user-attachments/assets/09eb989c-2a66-4225-b15a-2ded2d9299ee">


- **Transient Analysis for Propagation Delay**: SPICE command for pulse input:
   ``` 
   Vin in 0 0 pulse 0 2.5 0 10p 10p 1n 2n 
   .op 
   .tran 10p 4n 
   ```

    <img width="884" alt="Screenshot 2024-11-12 at 11 28 02 PM" src="https://github.com/user-attachments/assets/4f45f9d0-43ab-494e-8e18-c775f27509ae">

#### Cloning the Custom Inverter Layout

1. Clone the inverter:
   ``` 
   cd Desktop/work/tools/openlane_working_dir/openlane 
   git clone https://github.com/nickson-jose/vsdstdcelldesign 
   cd vsdstdcelldesign 
   cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech . 
   magic -T sky130A.tech sky130_inv.mag &
   ```

     ![image](https://github.com/user-attachments/assets/f699f53e-3714-494a-8d78-e6cd9f04cb0b)
  
#### CMOS Fabrication Process (16 Masks)

1. **Substrate Preparation**: The process begins with silicon wafer preparation as the base for the circuit.

2. **N-Well Formation**: Phosphorus impurities are implanted or diffused to create N-well regions on the substrate.

3. **P-Well Formation**: P-well regions are formed similarly, using boron or other dopants through ion implantation or diffusion.

4. **Gate Oxide Deposition**: A thin layer of silicon dioxide is deposited to act as the gate insulator.

5. **Polysilicon Deposition**: A polysilicon layer is deposited on the gate oxide to form the gate electrode.

6. **Polysilicon Masking and Etching**: Areas where polysilicon should remain are defined with a photoresist mask, and unprotected sections are etched away.

7. **N-Well Masking and Implantation**: A mask preserves N-well regions while phosphorus or similar impurities are implanted in other areas.

8. **P-Well Masking and Implantation**: A mask protects P-well regions, allowing boron or equivalent dopants to be implanted elsewhere.

9. **Source/Drain Implantation**: Using masks, dopants (e.g., arsenic for NMOS, boron for PMOS) are implanted to form the source and drain regions.

10. **Gate Formation**: The gate electrode is defined by etching the polysilicon layer, guided by a mask.

11. **Source/Drain Masking and Etching**: Masks define the source and drain areas, followed by etching to clear the oxide from those regions.

12. **Contact/Via Formation**: Holes are etched through the oxide layer to expose areas like the source/drain or polysilicon gates for contacts.

13. **Metal Deposition**: A metal layer, usually aluminum or copper, is deposited to create electrical interconnections.

14. **Metal Masking and Etching**: Metal interconnect patterns are defined using a photoresist mask, with etching to remove unneeded metal.

15. **Passivation Layer Deposition**: A protective silicon dioxide or nitride layer is applied to shield metal interconnections.

16. **Final Testing and Packaging**: Rigorous testing ensures chip functionality, after which functional chips are separated, packaged, and prepared for deployment.

    <img width="773" alt="Screenshot 2024-11-12 at 11 46 46 PM" src="https://github.com/user-attachments/assets/1b5b48f3-ee2d-47a5-8ebd-41614f33b050">


    ![image](https://github.com/user-attachments/assets/af12f629-ae30-470b-b7f3-7907a56c304c)
    ![image](https://github.com/user-attachments/assets/552f4cb7-b803-4ed3-93f8-9b296fc15f34)
    ![image](https://github.com/user-attachments/assets/71c6f0e2-e209-440a-9266-4926aa599824)
    ![image](https://github.com/user-attachments/assets/4951a329-06e7-473c-8513-efeb3552f48a)
    ![image](https://github.com/user-attachments/assets/814d4027-e61e-4a3e-bede-5bb62ad2405e)

    
#### SPICE Extraction with Magic

Run commands in `tkcon` to extract and generate SPICE:
``` 
pwd 
extract all 
ext2spice cthresh 0 rthresh 0 
ext2spice 
```
![image](https://github.com/user-attachments/assets/f4d29c39-488c-4cb3-830a-13033c23da33)
![image](https://github.com/user-attachments/assets/8b38ddb5-dc16-4a71-863c-e3a4a40a2cac)



#### Modifying SPICE File for Transient Analysis

![image](https://github.com/user-attachments/assets/e54e76fe-4f8d-4ac0-9d03-21baf96827ad)
![image](https://github.com/user-attachments/assets/2381ea43-ca61-49db-9bc7-28cc0714a9b6)


Edit `sky130_inv.spice`:
```
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

To simulate:
``` 
ngspice sky130_inv.spice 
plot y vs time a 
```
![image](https://github.com/user-attachments/assets/909c4e36-9228-4373-8c32-a11bd2aac4ae)
![image](https://github.com/user-attachments/assets/5d954379-a56d-4634-9725-5467ace37757)


#### Characterizing Slew Rate and Propagation Delay

Using transient response:
- **Rise Transition**: Time for output to rise from 20% to 80% of max value.
- **Fall Transition**: Time for output to fall from 80% to 20%.
- **Cell Rise/Fall Delay**: Difference in time for 50% output change compared to input transition.

Example calculations:
```
Rise Transition : 2.24638 - 2.18242 =  0.06396 ns = 63.96 ps
Fall Transition : 4.0955 - 4.05536 =  0.0419 ns = 41.9 ps
Cell Rise Delay : 2.21144 - 2.15008 = 0.06136 ns = 61.36 ps
Cell Fall Delay : 4.07807 - 4.05 =0.02 ns = 20 ps
```

#### Magic Tool DRC Rules Check

Set up and run:
``` 
cd 
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz 
tar xfz drc_tests.tgz 
cd drc_tests 
gvim .magicrc 
magic -d XR & 
```


Run DRC commands:
``` 
tech load sky130A.tech 
drc check 
drc why 
```

![image](https://github.com/user-attachments/assets/f641e032-850f-48ed-9b72-47b3821a0948)

![image](https://github.com/user-attachments/assets/4ebf7ecb-94be-4637-be32-5ad6782a858d)

![image](https://github.com/user-attachments/assets/94f66fa0-6734-41ed-8d66-3fe5c54cd783)

</details>

<details>
<summary><strong>Day-4:</strong> pre-layout timing analysis and importance of good clock tree </summary>

## pre-layout timing analysis and importance of good clock tree

Commands to extract `tracks.info` file:

```
cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign
cd ../../pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd/
less tracks.info
```
![image](https://github.com/user-attachments/assets/af2806cb-aa88-4b4f-b9c4-ef60024418ea)

Commands for tkcon window to set grid as tracks of locali layer

```
grid 0.46um 0.34um 0.23um 0.17um
```

![image](https://github.com/user-attachments/assets/0b747adf-64bf-4fa8-b3f8-9acc22538777)

Now, save it by giving a custom name

```
save sky130_karinv.mag
```
![image](https://github.com/user-attachments/assets/b7563e36-21b5-4e1e-8d55-0b77683eaed1)


Now, open it by using the following commands:

```
magic -T sky130A.tech sky130_karinv.mag &
```

Now, type the following command in tkcon window:

```
lef write
```

![image](https://github.com/user-attachments/assets/dd5cdfbb-c5a1-4669-9f1b-47756f0a325f)

Modify config.tcl:

```
# Design
set ::env(DESIGN_NAME) "picorv32a"

set ::env(VERILOG_FILES) "./designs/picorv32a/src/picorv32a.v"
set ::env(SDC_FILE) "./designs/picorv32a/src/picorv32a.sdc"

set ::env(CLOCK_PERIOD) "5.000"
set ::env(CLOCK_PORT) "clk"

set ::env(CLOCK_NET) $::env(CLOCK_PORT)

set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"

set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]

set filename $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/$::env(PDK)_$::env(STD_CELL_LIBRARY)_config.tcl
if { [file exists $filename] == 1 } {
  source $filename
}
```

Now, run openlane flow synthesis:

```
cd Desktop/work/tools/openlane_working_dir/openlane
docker
```

```
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
run_synthesis
```

![image](https://github.com/user-attachments/assets/618e5fd4-6ef8-4f9d-a713-1c710c6841cc)
![image](https://github.com/user-attachments/assets/d72dfd66-d0c0-4b4f-9efb-f8eac05ac216)
![image](https://github.com/user-attachments/assets/5dacb40a-b95f-408f-8920-8cc31d0c5ad5)

![image](https://github.com/user-attachments/assets/99f2ed08-91a2-4686-90f0-fdc2cd5cf244)



**Delay Tables**

Delay plays a crucial role in cell timing, impacted by input transition and output load. Cells of the same type can have different delays depending on wire length due to resistance and capacitance variations. To manage this, "delay tables" are created, using 2D arrays with input slew and load capacitance for each buffer size as timing models. Algorithms compute buffer delays from these tables, interpolating where exact data isn’t available to estimate delays accurately, preserving signal integrity across varying load conditions.

![image](https://github.com/user-attachments/assets/095a59e1-158c-4870-88e3-b73cb3a3692c)

Fixing slack:

```
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a -tag 13-11_12-30 -overwrite
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
echo $::env(SYNTH_STRATEGY)
set ::env(SYNTH_STRATEGY) "DELAY 3"
echo $::env(SYNTH_BUFFERING
echo $::env(SYNTH_SIZING)
set ::env(SYNTH_SIZING) 1
echo $::env(SYNTH_DRIVING_CELL)
run_synthesis
```

![image](https://github.com/user-attachments/assets/01104083-62d4-4b76-ae64-9b73efc48b75)
![image](https://github.com/user-attachments/assets/2a4e6be2-f445-40bd-bd65-eafdf0c9ab8d)
![image](https://github.com/user-attachments/assets/246fa577-df4d-48ac-800e-aaa5f60652e2)


Now, run floorplan

```
run_floorplan
```

![image](https://github.com/user-attachments/assets/8fe038e6-e3df-4722-b1a4-701c3ec025e5)

Since we are facing unexpected un-explainable error while using run_floorplan command, we can instead use the following set of commands available based on information from `Desktop/work/tools/openlane_working_dir/openlane/scripts/tcl_commands/floorplan.tcl` and also based on Floorplan Commands section in `Desktop/work/tools/openlane_working_dir/openlane/docs/source/OpenLANE_commands.md`

```
init_floorplan
place_io
tap_decap_or
```

Now, do placement

```
run_placement
```
![image](https://github.com/user-attachments/assets/6234cd36-f963-4491-a79d-c0ede87c1141)
![image](https://github.com/user-attachments/assets/76f3069b-b0c5-4b9b-b9af-f70d12948f5f)


Now, open a new terminal and run the below commands to load placement def in magic

```
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_12-30/results/placement/
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &

```

Custom inverter inserted in placement def

![image](https://github.com/user-attachments/assets/798c2ece-146b-476c-86ff-2f765722c412)

Now, select the cell and type `expand` in tkcon window to view internal layers of cells

![image](https://github.com/user-attachments/assets/e75cfacd-19c2-46f6-a4d3-3a7261ffa560)

**Timing analysis with ideal clocks using openSTA**

Pre-layout STA will include effects of clock buffers and net-delay due to RC parasitics (wire delay will be derived from PDK library wire model).

![image](https://github.com/user-attachments/assets/a74af227-70dd-4812-930d-b6e9e787a27f)

Since we are getting 0 wns after improved timing run, we will be doing the timing analysis on initial run of synthesis which has lots of violations and no parameters added to improve timing.

Commands to invoke the OpenLANE flow include new lef and perform synthesis:

```
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

Go, to `Desktop/work/tools/openlane_working_dir/openlane` and create a file `pre_sta.conf`. The contents of the file are:

```
set_cmd_units -time ns -capacitance pF -current mA -voltage V -resistance kOhm -distance um
read_liberty -max /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib
read_liberty -min /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib
read_verilog /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_12-30/results/synthesis/picorv32a.synthesis.v
link_design picorv32a
read_sdc /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/my_base.sdc
report_checks -path_delay min_max -fields {slew trans net cap input_pin}
report_tns
report_wns
```

Contents of `my_base.sdc`:

```
set ::env(CLOCK_PORT) clk
set ::env(CLOCK_PERIOD) 12.000
set ::env(SYNTH_DRIVING_CELL) sky130_fd_sc_hd__inv_8
set ::env(SYNTH_DRIVING_CELL_PIN) Y
set ::env(SYNTH_CAP_LOAD) 17.65
create_clock [get_ports $::env(CLOCK_PORT)]  -name $::env(CLOCK_PORT)  -period $::env(CLOCK_PERIOD)
set IO_PCT  0.2
set input_delay_value [expr $::env(CLOCK_PERIOD) * $IO_PCT]
set output_delay_value [expr $::env(CLOCK_PERIOD) * $IO_PCT]
puts "\[INFO\]: Setting output delay to: $output_delay_value"
puts "\[INFO\]: Setting input delay to: $input_delay_value"


set clk_indx [lsearch [all_inputs] [get_port $::env(CLOCK_PORT)]]
#set rst_indx [lsearch [all_inputs] [get_port resetn]]
set all_inputs_wo_clk [lreplace [all_inputs] $clk_indx $clk_indx]
#set all_inputs_wo_clk_rst [lreplace $all_inputs_wo_clk $rst_indx $rst_indx]
set all_inputs_wo_clk_rst $all_inputs_wo_clk


# correct resetn
set_input_delay $input_delay_value  -clock [get_clocks $::env(CLOCK_PORT)] $all_inputs_wo_clk_rst
#set_input_delay 0.0 -clock [get_clocks $::env(CLOCK_PORT)] {resetn}
set_output_delay $output_delay_value  -clock [get_clocks $::env(CLOCK_PORT)] [all_outputs]

# TODO set this as parameter
set_driving_cell -lib_cell $::env(SYNTH_DRIVING_CELL) -pin $::env(SYNTH_DRIVING_CELL_PIN) [all_inputs]
set cap_load [expr $::env(SYNTH_CAP_LOAD) / 1000.0]
puts "\[INFO\]: Setting load to: $cap_load"
set_load  $cap_load [all_outputs]
```
Commands to run STA:

```
cd Desktop/work/tools/openlane_working_dir/openlane
sta pre_sta.conf
```

![image](https://github.com/user-attachments/assets/8e662e9f-15bd-4c4e-b973-444320c7da04)
![image](https://github.com/user-attachments/assets/5680de47-878d-4113-bce3-1450436309aa)


We now try to optimise synthesis.

Go to new terminal and run the following commands:

```
cd Desktop/work/tools/openlane_working_dir/openlane
docker
./flow.tcl -interactive
prep -design picorv32a -tag 13-11_12-30 -overwrite
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
set ::env(SYNTH_SIZING) 1
set ::env(SYNTH_MAX_FANOUT) 4
echo $::env(SYNTH_DRIVING_CELL)
run_synthesis
```

![image](https://github.com/user-attachments/assets/8bb0421d-0679-4b0d-8246-a6ce80a27d4e)


Commands to run STA:

```
cd Desktop/work/tools/openlane_working_dir/openlane
sta pre_sta.conf
```
![image](https://github.com/user-attachments/assets/5680de47-878d-4113-bce3-1450436309aa)


**Basic timing ECO**

NOR gate of drive strength 2 is driving 5 fanouts

![image](https://github.com/user-attachments/assets/730911f7-81ca-4a4c-bb76-6267094e1be1)

Run the following commands to optimise timing:

```
report_net -connections _13111_
replace_cell _16171_ sky130_fd_sc_hd__nor3_2
report_checks -fields {net cap slew input_pins} -digits 4
```

![image](https://github.com/user-attachments/assets/d38d4a2c-5847-473a-b436-9f7aca504d2b)

We can observe that the tns has reduced to -402.45 from -403.54 and wns has reduced to -5.44 from -5.59

**Clock tree synthesis TritonCTS and signal integrity**

Clock Tree Synthesis (CTS) techniques vary based on design needs:

- Balanced Tree CTS: Uses a balanced binary-like tree for equal path lengths, minimizing clock skew but with moderate power efficiency.
- H-tree CTS: Employs an "H"-shaped structure, good for large areas and power efficiency.

   ![image](https://github.com/user-attachments/assets/d1b13f19-a87f-41b6-8f29-a4e00a8e7216)

- Star CTS: Distributes the clock from a central point, minimizing skew but requiring more buffers near the source.
- Global-Local CTS: Combines star and tree topologies, with a global tree for clock domains and local trees within domains, balancing global and local timing.
- Mesh CTS: Uses a grid pattern ideal for structured designs, balancing simplicity and skew.
- Adaptive CTS: Dynamically adjusts based on timing and congestion, offering flexibility but with added complexity.

**Crosstalk**

Crosstalk is interference from overlapping electromagnetic fields between adjacent circuits, causing unwanted signals. In VLSI, it can lead to data corruption, timing issues, and higher power consumption. Mitigation strategies include optimized layout and routing, shielding, and clock gating to reduce dynamic power and minimize crosstalk effects.

![image](https://github.com/user-attachments/assets/21df4ac0-57aa-492e-adfa-7e04ce385680)

**Clock Net Shielding**

Clock net shielding prevents glitches by isolating the clock network, using shields connected to VDD or GND that don’t switch. It reduces interference by isolating clocks from other signals, often with dedicated routing layers and clock buffers. Additionally, clock domain isolation helps prevent cross-domain interference, avoiding metastability and maintaining synchronization.

![image](https://github.com/user-attachments/assets/bf85dd84-dc29-4962-877a-ce4f535bab2c)

Now to insert this updated netlist to PnR flow and we can use write_verilog and overwrite the synthesis netlist but before that we are going to make a copy of the old old netlist:

Run the following commands:

```
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_12-30/results/synthesis/
ls
cp picorv32a.synthesis.v picorv32a.synthesis_old.v
ls
```

Commands to write verilog:

```
write_verilog /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_12-30/results/synthesis/picorv32a.synthesis.v
exit
```
Verified that the netlist is overwritten

![image](https://github.com/user-attachments/assets/432ba6a1-1670-439c-ae0c-28fd1b11d1dd)

Now, run the following commands:

```
cd Desktop/work/tools/openlane_working_dir/openlane
docker
./flow.tcl -interactive
prep -design picorv32a -tag 13-11_12-30 -overwrite
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

![image](https://github.com/user-attachments/assets/f9e530bf-dba9-4496-bf74-97bd8b1817e2)
![image](https://github.com/user-attachments/assets/42a95018-2966-43ce-b926-d3c3144c7bcd)
![image](https://github.com/user-attachments/assets/bee24a34-35d8-47b0-b067-1c880e68300d)

**Setup timing analysis using real clocks**

A real clock in timing analysis accounts for practical factors like clock skew and clock jitter. Clock skew is the difference in arrival times of the clock signal at different parts of the circuit due to physical delays, which affects setup and hold timing margins. Clock jitter is the variability in the clock period caused by power, temperature, and noise fluctuations, leading to uncertainty in clock edge timing. Both factors are crucial for accurate timing analysis, ensuring the design performs reliably in real-world conditions.

![image](https://github.com/user-attachments/assets/3526c927-e1a9-445a-9dae-22bc7e0446c7)

![image](https://github.com/user-attachments/assets/0c766405-5f9b-4700-a4cd-6fd19e9ea6cc)

Now, enter the following commands for Post-CTS OpenROAD timing analysis:

```
openroad
read_lef /openLANE_flow/designs/picorv32a/runs/13-11_12-30/tmp/merged.lef
read_def /openLANE_flow/designs/picorv32a/runs/13-11_12-30/results/cts/picorv32a.cts.def
write_db pico_cts.db
read_db pico_cts.db
read_verilog /openLANE_flow/designs/picorv32a/runs/13-11_12-30/results/synthesis/picorv32a.synthesis_cts.v
read_liberty $::env(LIB_SYNTH_COMPLETE)
link_design picorv32a
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc
set_propagated_clock [all_clocks]
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4
exit
```

![image](https://github.com/user-attachments/assets/05510efc-bd04-4530-8753-6f566218dd7d)
![image](https://github.com/user-attachments/assets/2ace586d-3484-40bf-9269-58a0fde79314)

Now, enter the following commands for exploring post-CTS OpenROAD timing analysis by removing 'sky130_fd_sc_hd__clkbuf_1' cell from clock buffer list variable 'CTS_CLK_BUFFER_LIST':

```
echo $::env(CTS_CLK_BUFFER_LIST)
set ::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0]
echo $::env(CTS_CLK_BUFFER_LIST)
echo $::env(CURRENT_DEF)
set ::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/13-11_12-30/results/placement/picorv32a.placement.def
run_cts
echo $::env(CTS_CLK_BUFFER_LIST)
openroad
read_lef /openLANE_flow/designs/picorv32a/runs/13-11_12-30/tmp/merged.lef
read_def /openLANE_flow/designs/picorv32a/runs/13-11_12-30/results/cts/picorv32a.cts.def
write_db pico_cts1.db
read_db pico_cts.db
read_verilog /openLANE_flow/designs/picorv32a/runs/13-11_12-30/results/synthesis/picorv32a.synthesis_cts.v
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

![image](https://github.com/user-attachments/assets/245a4ac8-0bd4-456a-95ee-7fddad23c431)
![image](https://github.com/user-attachments/assets/73348291-6944-4631-a7b2-580f2ef4cf65)

</details>
<details>
	
<summary><strong>Day-5:</strong> Final steps for RTL2GDS using tritonRoute and openSTA </summary>

##  Final steps for RTL2GDS using tritonRoute and openSTA 

### 1. Perform generation of Power Distribution Network (PDN) and explore the PDN layout.

Commands to perform all necessary stages up until now
```

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
Screenshots of power distribution network run

![image](https://github.com/user-attachments/assets/af5d2398-6077-400b-9805-003a5380c46f)
![image](https://github.com/user-attachments/assets/3ad4609e-f123-4ca5-bee8-6d17ccf1c78c)
![image](https://github.com/user-attachments/assets/2385a045-df72-442c-9c92-840a251cc23d)
![image](https://github.com/user-attachments/assets/1529d6ca-2037-4b11-bd88-cc72ecc99770)

<img width="993" alt="Screenshot 2024-11-14 at 1 14 54 AM" src="https://github.com/user-attachments/assets/9a2e38bd-9cae-47d2-9415-49884658d545">

<img width="1010" alt="Screenshot 2024-11-14 at 1 15 02 AM" src="https://github.com/user-attachments/assets/df16bfeb-33a4-488d-96b6-d9d9d33e9077">


## 2. Perfrom detailed routing using TritonRoute and explore the routed layout.
Command to perform routing
```
echo $::env(CURRENT_DEF)
echo $::env(ROUTING_STRATEGY)
run_routing
```
Screenshots of routing run

![image](https://github.com/user-attachments/assets/b3f46508-98e1-462c-a320-dff132653532)
![image](https://github.com/user-attachments/assets/13e40893-459e-4f69-bfa2-1c9068baaf29)


<img width="1007" alt="Screenshot 2024-11-14 at 1 15 24 AM" src="https://github.com/user-attachments/assets/507ba59e-e8ce-4c0d-8d42-3f5d6b26c857">

<img width="623" alt="Screenshot 2024-11-14 at 1 18 40 AM" src="https://github.com/user-attachments/assets/975f23b9-3129-40d0-915d-2a12fde2df7d">

Screenshot of fast route guide present in openlane/designs/picorv32a/runs/13-11_12-30/tmp/routing directory

![image](https://github.com/user-attachments/assets/e7ed2db7-8f50-4960-bbaf-9222ac08f98f)




## 3. Post-Route parasitic extraction using SPEF extractor.
Commands for SPEF extraction using external tool
```c

cd Desktop/work/tools/SPEF_EXTRACTOR

python3 main.py /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_12-30/tmp/merged.lef /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_18-24/results/routing/picorv32a.def
```

## 4. Post-Route OpenSTA timing analysis with the extracted parasitics of the route.
Commands to be run in OpenLANE flow to do OpenROAD timing analysis with integrated OpenSTA in OpenROAD
```
openroad
read_lef /openLANE_flow/designs/picorv32a/runs/13-11_12-30/tmp/merged.lef
read_def /openLANE_flow/designs/picorv32a/runs/13-11_12-30/results/routing/picorv32a.def
write_db pico_route.db
read_db pico_route.db
read_verilog /openLANE_flow/designs/picorv32a/runs/13-11_12-30/results/synthesis/picorv32a.synthesis_preroute.v
read_liberty $::env(LIB_SYNTH_COMPLETE)
link_design picorv32a
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc
set_propagated_clock [all_clocks]
read_spef /openLANE_flow/designs/picorv32a/runs/13-11_12-30/results/routing/picorv32a.spef
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4
exit

```

</details>
</details>

<details><summary><strong>Lab14</strong></summary>

## Installing and setting up ORFS:

```c

git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts
cd OpenROAD-flow-scripts
sudo ./setup.sh

```


![image](https://github.com/user-attachments/assets/36f750fb-3251-44f6-aec8-753704b1477d)


![image](https://github.com/user-attachments/assets/0ac8a1d2-ce4c-45c9-b310-a2eb204daee2)



```c
./build_openroad.sh --local
```

![image](https://github.com/user-attachments/assets/63efe589-3e2d-42c6-a4cc-990d250a238a)

## Verify Installation

```c
source ./env.sh
yosys -help
openroad -help
cd flow
make
```

![ss1](https://github.com/user-attachments/assets/1562f2c4-84e0-42d1-8e67-5bd013e64c28)

![image](https://github.com/user-attachments/assets/789b42c0-251b-4aea-b971-0f761826297c)


```c
make gui_final
```

![image](https://github.com/user-attachments/assets/17e41a9b-8522-4176-a9d6-e039631286db)


## ORFS Directory Structure and File formats:

![image](https://github.com/user-attachments/assets/8e72c9dc-e190-4b4f-8514-9bf488fe3e2d)

```c
├── OpenROAD-flow-scripts             
│   ├── docker           -> It has Docker based installation, run scripts and all saved here
│   ├── docs             -> Documentation for OpenROAD or its flow scripts.  
│   ├── flow             -> Files related to run RTL to GDS flow  
|   ├── jenkins          -> It contains the regression test designed for each build update
│   ├── tools            -> It contains all the required tools to run RTL to GDS flow
│   ├── etc              -> Has the dependency installer script and other things
│   ├── setup_env.sh     -> Its the source file to source all our OpenROAD rules to run the RTL to GDS flow
```

Navigate to flow directory

![image](https://github.com/user-attachments/assets/94563d18-9ea8-4f5f-9a3c-b41460ab68af)

```c
├── flow           
│   ├── design           -> It has built-in examples from RTL to GDS flow across different technology nodes
│   ├── makefile         -> The automated flow runs through makefile setup
│   ├── platform         -> It has different technology note libraries, lef files, GDS etc 
|   ├── tutorials        
│   ├── util            
│   ├── scripts
```


## Automated RTL-to-GDS Flow for VSDBabySoC

### Initial Setup Steps

1. **Create the `vsdbabysoc` Directory**  
   Create a directory named `vsdbabysoc` inside the path `OpenROAD-flow-scripts/flow/designs/sky130hd`.

2. **Copy Necessary Folders and Files**  
   Copy the following folders from the VSDBabySoC directory on your system into the `vsdbabysoc` directory:

   - **`gds` Folder**  
     Ensure it contains:
     - `avsddac.gds`
     - `avsdpll.gds`

   - **`include` Folder**  
     Ensure it contains:
     - `sandpiper.vh`
     - `sandpiper_gen.vh`
     - `sp_default.vh`
     - `sp_verilog.vh`

   - **`lef` Folder**  
     Ensure it contains:
     - `avsddac.lef`
     - `avsdpll.lef`

   - **`lib` Folder**  
     Ensure it contains:
     - `avsddac.lib`
     - `avsdpll.lib`

3. **Copy Constraints File**  
   Copy the constraints file `vsdbabysoc_synthesis.sdc` from the VSDBabySoC directory on your system into the `vsdbabysoc` directory.

4. **Copy Configuration Files**  
   Copy the following configuration files from the VSDBabySoC directory on your system into the `vsdbabysoc` directory:
   - `macro.cfg`
   - `pin_order.cfg`

5. **Create a `macro.cfg` File**  
   Create a `macro.cfg` file in the `vsdbabysoc` directory with the following content:
```c
export DESIGN_NICKNAME = vsdbabysoc
export DESIGN_NAME = vsdbabysoc
export PLATFORM    = sky130hd

# export VERILOG_FILES_BLACKBOX = $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/IPs/*.v
# export VERILOG_FILES = $(sort $(wildcard $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/*.v))
# Explicitly list the Verilog files for synthesis
export VERILOG_FILES = $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/vsdbabysoc.v \
                       $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/rvmyth.v \
                       $(DESIGN_HOME)/src/$(DESIGN_NICKNAME)/clk_gate.v

export SDC_FILE      = $(DESIGN_HOME)/$(PLATFORM)/$(DESIGN_NICKNAME)/vsdbabysoc_synthesis.sdc

export vsdbabysoc_DIR = $(DESIGN_HOME)/$(PLATFORM)/$(DESIGN_NICKNAME)

export VERILOG_INCLUDE_DIRS = $(wildcard $(vsdbabysoc_DIR)/include/)
# export SDC_FILE      = $(wildcard $(vsdbabysoc_DIR)/sdc/*.sdc)
export ADDITIONAL_GDS  = $(wildcard $(vsdbabysoc_DIR)/gds/*.gds.gz)
export ADDITIONAL_LEFS  = $(wildcard $(vsdbabysoc_DIR)/lef/*.lef)
export ADDITIONAL_LIBS = $(wildcard $(vsdbabysoc_DIR)/lib/*.lib)
# export PDN_TCL = $(DESIGN_HOME)/$(PLATFORM)/$(DESIGN_NICKNAME)/pdn.tcl

# Clock Configuration (vsdbabysoc specific)
# export CLOCK_PERIOD = 20.0
export CLOCK_PORT = CLK
export CLOCK_NET = $(CLOCK_PORT)

# Floorplanning Configuration (vsdbabysoc specific)
export FP_PIN_ORDER_CFG = $(wildcard $(DESIGN_DIR)/pin_order.cfg)
# export FP_SIZING = absolute

export DIE_AREA   = 0 0 1600 1600
export CORE_AREA  = 20 20 1590 1590

# Placement Configuration (vsdbabysoc specific)
export MACRO_PLACEMENT_CFG = $(wildcard $(DESIGN_DIR)/macro.cfg)
export PLACE_PINS_ARGS = -exclude left:0-600 -exclude left:1000-1600: -exclude right:* -exclude top:* -exclude bottom:*
# export MACRO_PLACEMENT = $(DESIGN_HOME)/$(PLATFORM)/$(DESIGN_NICKNAME)/macro_placement.cfg

export TNS_END_PERCENT = 100
export REMOVE_ABC_BUFFERS = 1

# Magic Tool Configuration
export MAGIC_ZEROIZE_ORIGIN = 0
export MAGIC_EXT_USE_GDS = 1

# CTS tuning
export CTS_BUF_DISTANCE = 600
export SKIP_GATE_CLONING = 1

# export CORE_UTILIZATION=0.1  # Reduce this value to allow more whitespace for routing.
```

Now go to terminal and run the following commands:
```c


cd OpenROAD-flow-scripts
source env.sh
cd flow

```
Run the following command for synthesis:
```c
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk synth
```

![image](https://github.com/user-attachments/assets/df09e84f-b7a0-4014-91fd-6c87b7544c41)


![image](https://github.com/user-attachments/assets/2edfa8ce-c78b-4d2b-8af2-10e42e5c5699)


Synthesis netlist:

![image](https://github.com/user-attachments/assets/b5442a19-8e0d-42ed-871a-f19a1467bbed)


Synthesis log:

![image](https://github.com/user-attachments/assets/1e5131c2-4a61-4ef7-a883-608014252167)

Synthesis Check:

![image](https://github.com/user-attachments/assets/9887aae0-c63f-4987-826c-2825643c7c17)

Synthesis Stats:

![image](https://github.com/user-attachments/assets/c517e77f-b4a5-4e5f-b0f6-305f1f1f7876)

![image](https://github.com/user-attachments/assets/0dc4ed79-a90f-4b1e-a139-270ade54ac35)

![image](https://github.com/user-attachments/assets/13ef3589-79ae-41c4-9ebb-0259367b33e9)

![image](https://github.com/user-attachments/assets/ce69fd69-8de3-4180-9e57-7493bfc6f52e)

### Run the following commands for floorplan:

```c
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk floorplan
```

![image](https://github.com/user-attachments/assets/be1d1cba-7400-42fe-adb8-e8af4f6697a9)

![image](https://github.com/user-attachments/assets/873ea0e9-cbc6-48d0-a1a3-d7567e8272ca)



```c
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_floorplan
```

![image](https://github.com/user-attachments/assets/e883647f-84c0-4625-aa93-d023988fdfce)

![image](https://github.com/user-attachments/assets/8e028ee3-42a7-4b2c-82b1-d7511a403a0e)


### Run the following commands for place:
```c
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk place
```

![image](https://github.com/user-attachments/assets/184098f7-a905-4f4a-93ce-58705557d425)
![image](https://github.com/user-attachments/assets/65da9da4-74b1-4fb1-b234-c3814b87d20c)



```c
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_place
```

![image](https://github.com/user-attachments/assets/ec629aec-cf85-4104-9103-ae529be06960)

![image](https://github.com/user-attachments/assets/f8729685-159f-4e9c-b9b1-5aa67edae327)

### HeatMap:

![image](https://github.com/user-attachments/assets/48d084c2-f523-4ae1-8fc1-008bb5ed7567)

![image](https://github.com/user-attachments/assets/b0256948-c1b6-4d64-b685-a91f1b2b519a)

![image](https://github.com/user-attachments/assets/d77e4ecf-375e-4e87-adfc-edf1f58bcfff)


### Run the following commands for cts:

```c
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk cts
```

![image](https://github.com/user-attachments/assets/f6c90a08-b108-4cb6-9f7a-e2ce03d88a7f)

![image](https://github.com/user-attachments/assets/40c183ec-1c8b-4efb-a18c-e585380447e6)



```c
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_cts
```
![image](https://github.com/user-attachments/assets/12f1b8fb-2a00-4fe1-8cfc-6e68d27cc3b3)

![image](https://github.com/user-attachments/assets/395e20be-5231-4a54-9d4d-5d242eeebeb0)


### CTS Final Report:

![image](https://github.com/user-attachments/assets/11e6b9ee-f3a4-460c-bec9-8066853f14a1)



```c

==========================================================================
cts final report_tns
--------------------------------------------------------------------------
tns 0.00

==========================================================================
cts final report_wns
--------------------------------------------------------------------------
wns 0.00

==========================================================================
cts final report_worst_slack
--------------------------------------------------------------------------
worst slack 4.62

==========================================================================
cts final report_clock_skew
--------------------------------------------------------------------------
Clock clk
   0.93 source latency core.CPU_reset_a4$_DFF_P_/CLK ^
  -0.84 target latency core.CPU_Dmem_value_a5[9][20]$_SDFFE_PP0P_/CLK ^
   0.54 clock uncertainty
   0.00 CRPR
--------------
   0.63 setup skew


==========================================================================
cts final report_checks -path_delay min
--------------------------------------------------------------------------
Startpoint: core.CPU_dmem_rd_data_a5[26]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_Xreg_value_a4[13][26]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.30    0.00    0.00    0.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.04    0.02    0.02 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.35    0.35    0.35    0.38 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.35    0.00    0.38 ^ clkbuf_4_3_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     6    0.12    0.13    0.29    0.67 ^ clkbuf_4_3_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_3_0_CLK (net)
                  0.13    0.00    0.67 ^ clkbuf_leaf_7_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    10    0.04    0.06    0.17    0.84 ^ clkbuf_leaf_7_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_7_CLK (net)
                  0.06    0.00    0.84 ^ core.CPU_dmem_rd_data_a5[26]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_2)
     1    0.04    0.10    0.38    1.22 v core.CPU_dmem_rd_data_a5[26]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_2)
                                         core.CPU_dmem_rd_data_a5[26] (net)
                  0.10    0.00    1.22 v _10561_/B (sky130_fd_sc_hd__and3_4)
    31    0.12    0.16    0.31    1.54 v _10561_/X (sky130_fd_sc_hd__and3_4)
                                         _04903_ (net)
                  0.16    0.00    1.54 v _11102_/A (sky130_fd_sc_hd__nand2_1)
     1    0.00    0.06    0.10    1.64 ^ _11102_/Y (sky130_fd_sc_hd__nand2_1)
                                         _05344_ (net)
                  0.06    0.00    1.64 ^ _11103_/C1 (sky130_fd_sc_hd__o211ai_1)
     1    0.00    0.05    0.07    1.71 v _11103_/Y (sky130_fd_sc_hd__o211ai_1)
                                         _00658_ (net)
                  0.05    0.00    1.71 v core.CPU_Xreg_value_a4[13][26]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  1.71   data arrival time

                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.30    0.00    0.00    0.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.04    0.02    0.02 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.35    0.35    0.35    0.38 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.35    0.00    0.38 ^ clkbuf_4_11_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    12    0.18    0.19    0.33    0.71 ^ clkbuf_4_11_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_11_0_CLK (net)
                  0.19    0.00    0.71 ^ clkbuf_leaf_122_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    13    0.04    0.06    0.19    0.91 ^ clkbuf_leaf_122_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_122_CLK (net)
                  0.06    0.00    0.91 ^ core.CPU_Xreg_value_a4[13][26]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                          0.86    1.76   clock uncertainty
                          0.00    1.76   clock reconvergence pessimism
                         -0.05    1.71   library hold time
                                  1.71   data required time
-----------------------------------------------------------------------------
                                  1.71   data required time
                                 -1.71   data arrival time
-----------------------------------------------------------------------------
                                  0.00   slack (MET)



==========================================================================
cts final report_checks -path_delay max
--------------------------------------------------------------------------
Startpoint: core.CPU_valid_load_a5$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_Xreg_value_a4[5][0]$_SDFFE_PP1P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.30    0.00    0.00    0.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.04    0.02    0.02 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.35    0.35    0.35    0.38 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.35    0.00    0.38 ^ clkbuf_4_7_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    11    0.17    0.19    0.33    0.71 ^ clkbuf_4_7_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_7_0_CLK (net)
                  0.19    0.00    0.71 ^ clkbuf_leaf_54_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    14    0.04    0.06    0.19    0.90 ^ clkbuf_leaf_54_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_54_CLK (net)
                  0.06    0.00    0.90 ^ core.CPU_valid_load_a5$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_2)
     3    0.04    0.18    0.42    1.33 ^ core.CPU_valid_load_a5$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_2)
                                         core.CPU_valid_load_a5 (net)
                  0.18    0.00    1.33 ^ _07947_/D (sky130_fd_sc_hd__or4_4)
    73    0.53    1.50    1.15    2.47 ^ _07947_/X (sky130_fd_sc_hd__or4_4)
                                         _02906_ (net)
                  1.50    0.06    2.53 ^ _07949_/A (sky130_fd_sc_hd__clkinv_16)
    90    0.63    0.62    0.75    3.28 v _07949_/Y (sky130_fd_sc_hd__clkinv_16)
                                         _02908_ (net)
                  0.62    0.03    3.31 v _11035_/C (sky130_fd_sc_hd__or3_2)
     2    0.02    0.12    0.69    4.01 v _11035_/X (sky130_fd_sc_hd__or3_2)
                                         _05295_ (net)
                  0.12    0.00    4.01 v _11036_/B1 (sky130_fd_sc_hd__o21a_4)
     8    0.12    0.17    0.27    4.28 v _11036_/X (sky130_fd_sc_hd__o21a_4)
                                         _05296_ (net)
                  0.17    0.00    4.28 v _13430_/B (sky130_fd_sc_hd__nor3_4)
    24    0.14    1.36    1.11    5.39 ^ _13430_/Y (sky130_fd_sc_hd__nor3_4)
                                         _06954_ (net)
                  1.36    0.01    5.39 ^ _13433_/A3 (sky130_fd_sc_hd__a31oi_1)
     1    0.00    0.16    0.17    5.56 v _13433_/Y (sky130_fd_sc_hd__a31oi_1)
                                         _06957_ (net)
                  0.16    0.00    5.56 v hold2286/A (sky130_fd_sc_hd__dlygate4sd3_1)
     1    0.00    0.05    0.60    6.16 v hold2286/X (sky130_fd_sc_hd__dlygate4sd3_1)
                                         net2394 (net)
                  0.05    0.00    6.16 v _13434_/B (sky130_fd_sc_hd__or2_0)
     1    0.00    0.05    0.19    6.35 v _13434_/X (sky130_fd_sc_hd__or2_0)
                                         _01376_ (net)
                  0.05    0.00    6.35 v core.CPU_Xreg_value_a4[5][0]$_SDFFE_PP1P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  6.35   data arrival time

                         10.70   10.70   clock clk (rise edge)
                          0.00   10.70   clock source latency
     1    0.30    0.00    0.00   10.70 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.04    0.02   10.72 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.35    0.35    0.35   11.08 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.35    0.00   11.08 ^ clkbuf_4_8_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    13    0.19    0.20    0.34   11.42 ^ clkbuf_4_8_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_8_0_CLK (net)
                  0.20    0.00   11.42 ^ clkbuf_leaf_160_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    11    0.04    0.06    0.19   11.61 ^ clkbuf_leaf_160_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_160_CLK (net)
                  0.06    0.00   11.61 ^ core.CPU_Xreg_value_a4[5][0]$_SDFFE_PP1P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.54   11.07   clock uncertainty
                          0.00   11.07   clock reconvergence pessimism
                         -0.11   10.96   library setup time
                                 10.96   data required time
-----------------------------------------------------------------------------
                                 10.96   data required time
                                 -6.35   data arrival time
-----------------------------------------------------------------------------
                                  4.62   slack (MET)



==========================================================================
cts final report_checks -unconstrained
--------------------------------------------------------------------------
Startpoint: core.CPU_valid_load_a5$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_Xreg_value_a4[5][0]$_SDFFE_PP1P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.30    0.00    0.00    0.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.04    0.02    0.02 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.35    0.35    0.35    0.38 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.35    0.00    0.38 ^ clkbuf_4_7_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    11    0.17    0.19    0.33    0.71 ^ clkbuf_4_7_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_7_0_CLK (net)
                  0.19    0.00    0.71 ^ clkbuf_leaf_54_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    14    0.04    0.06    0.19    0.90 ^ clkbuf_leaf_54_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_54_CLK (net)
                  0.06    0.00    0.90 ^ core.CPU_valid_load_a5$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_2)
     3    0.04    0.18    0.42    1.33 ^ core.CPU_valid_load_a5$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_2)
                                         core.CPU_valid_load_a5 (net)
                  0.18    0.00    1.33 ^ _07947_/D (sky130_fd_sc_hd__or4_4)
    73    0.53    1.50    1.15    2.47 ^ _07947_/X (sky130_fd_sc_hd__or4_4)
                                         _02906_ (net)
                  1.50    0.06    2.53 ^ _07949_/A (sky130_fd_sc_hd__clkinv_16)
    90    0.63    0.62    0.75    3.28 v _07949_/Y (sky130_fd_sc_hd__clkinv_16)
                                         _02908_ (net)
                  0.62    0.03    3.31 v _11035_/C (sky130_fd_sc_hd__or3_2)
     2    0.02    0.12    0.69    4.01 v _11035_/X (sky130_fd_sc_hd__or3_2)
                                         _05295_ (net)
                  0.12    0.00    4.01 v _11036_/B1 (sky130_fd_sc_hd__o21a_4)
     8    0.12    0.17    0.27    4.28 v _11036_/X (sky130_fd_sc_hd__o21a_4)
                                         _05296_ (net)
                  0.17    0.00    4.28 v _13430_/B (sky130_fd_sc_hd__nor3_4)
    24    0.14    1.36    1.11    5.39 ^ _13430_/Y (sky130_fd_sc_hd__nor3_4)
                                         _06954_ (net)
                  1.36    0.01    5.39 ^ _13433_/A3 (sky130_fd_sc_hd__a31oi_1)
     1    0.00    0.16    0.17    5.56 v _13433_/Y (sky130_fd_sc_hd__a31oi_1)
                                         _06957_ (net)
                  0.16    0.00    5.56 v hold2286/A (sky130_fd_sc_hd__dlygate4sd3_1)
     1    0.00    0.05    0.60    6.16 v hold2286/X (sky130_fd_sc_hd__dlygate4sd3_1)
                                         net2394 (net)
                  0.05    0.00    6.16 v _13434_/B (sky130_fd_sc_hd__or2_0)
     1    0.00    0.05    0.19    6.35 v _13434_/X (sky130_fd_sc_hd__or2_0)
                                         _01376_ (net)
                  0.05    0.00    6.35 v core.CPU_Xreg_value_a4[5][0]$_SDFFE_PP1P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  6.35   data arrival time

                         10.70   10.70   clock clk (rise edge)
                          0.00   10.70   clock source latency
     1    0.30    0.00    0.00   10.70 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.04    0.02   10.72 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.35    0.35    0.35   11.08 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.35    0.00   11.08 ^ clkbuf_4_8_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    13    0.19    0.20    0.34   11.42 ^ clkbuf_4_8_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_4_8_0_CLK (net)
                  0.20    0.00   11.42 ^ clkbuf_leaf_160_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    11    0.04    0.06    0.19   11.61 ^ clkbuf_leaf_160_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_160_CLK (net)
                  0.06    0.00   11.61 ^ core.CPU_Xreg_value_a4[5][0]$_SDFFE_PP1P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.54   11.07   clock uncertainty
                          0.00   11.07   clock reconvergence pessimism
                         -0.11   10.96   library setup time
                                 10.96   data required time
-----------------------------------------------------------------------------
                                 10.96   data required time
                                 -6.35   data arrival time
-----------------------------------------------------------------------------
                                  4.62   slack (MET)



==========================================================================
cts final report_check_types -max_slew -max_cap -max_fanout -violators
--------------------------------------------------------------------------
max slew

Pin                                    Limit    Slew   Slack
------------------------------------------------------------
_13158_/A                               1.50    1.51   -0.01 (VIOLATED)
_12836_/A                               1.50    1.51   -0.01 (VIOLATED)
_12427_/A                               1.50    1.51   -0.01 (VIOLATED)
_11369_/A                               1.50    1.51   -0.01 (VIOLATED)
_11681_/A                               1.50    1.51   -0.01 (VIOLATED)
_12127_/A                               1.50    1.51   -0.01 (VIOLATED)
_12525_/A                               1.50    1.51   -0.01 (VIOLATED)
_12328_/A                               1.50    1.51   -0.01 (VIOLATED)
_13060_/A                               1.50    1.51   -0.01 (VIOLATED)
_12628_/A                               1.50    1.51   -0.01 (VIOLATED)
_11578_/A                               1.50    1.51   -0.01 (VIOLATED)
_13757_/A                               1.50    1.51   -0.01 (VIOLATED)
_12731_/A                               1.50    1.51   -0.01 (VIOLATED)
_11473_/A                               1.50    1.51   -0.01 (VIOLATED)
_12028_/A                               1.50    1.51   -0.01 (VIOLATED)
_11912_/A                               1.50    1.51   -0.01 (VIOLATED)
_12227_/A                               1.50    1.51   -0.01 (VIOLATED)
_11168_/A                               1.50    1.51   -0.01 (VIOLATED)
_12946_/A                               1.50    1.51   -0.01 (VIOLATED)
_13858_/A                               1.50    1.51   -0.01 (VIOLATED)
_11268_/A                               1.50    1.51   -0.01 (VIOLATED)
_13256_/A                               1.50    1.51   -0.01 (VIOLATED)
_10301_/A                               1.50    1.51   -0.01 (VIOLATED)
_10958_/A                               1.50    1.51   -0.01 (VIOLATED)
_11790_/A                               1.50    1.51   -0.01 (VIOLATED)
_13558_/A                               1.50    1.51   -0.01 (VIOLATED)
_10834_/A                               1.50    1.51   -0.01 (VIOLATED)
_13658_/A                               1.50    1.51   -0.01 (VIOLATED)
_13357_/A                               1.50    1.51   -0.01 (VIOLATED)
_13459_/A                               1.50    1.51   -0.01 (VIOLATED)
_11067_/A                               1.50    1.51   -0.01 (VIOLATED)
_10303_/B                               1.50    1.50   -0.00 (VIOLATED)
_10561_/C                               1.50    1.50   -0.00 (VIOLATED)
_10538_/C                               1.50    1.50   -0.00 (VIOLATED)
_10409_/B                               1.50    1.50   -0.00 (VIOLATED)
_11036_/A1                              1.50    1.50   -0.00 (VIOLATED)
_07949_/A                               1.50    1.50   -0.00 (VIOLATED)
_10084_/B                               1.50    1.50   -0.00 (VIOLATED)
_10508_/B                               1.50    1.50   -0.00 (VIOLATED)
_11042_/A1                              1.50    1.50   -0.00 (VIOLATED)
_10232_/A1                              1.50    1.50   -0.00 (VIOLATED)
_10231_/B                               1.50    1.50   -0.00 (VIOLATED)
_09990_/C                               1.50    1.50   -0.00 (VIOLATED)
_09999_/C                               1.50    1.50   -0.00 (VIOLATED)
_11879_/A                               1.50    1.50   -0.00 (VIOLATED)
_10007_/B                               1.50    1.50   -0.00 (VIOLATED)
_10000_/A1                              1.50    1.50   -0.00 (VIOLATED)
_11880_/A3                              1.50    1.50   -0.00 (VIOLATED)
_10008_/A3                              1.50    1.50   -0.00 (VIOLATED)
_09996_/C                               1.50    1.50   -0.00 (VIOLATED)
_10799_/C                               1.50    1.50   -0.00 (VIOLATED)
_09997_/A3                              1.50    1.50   -0.00 (VIOLATED)
_10925_/A1                              1.50    1.50   -0.00 (VIOLATED)
_10095_/A1                              1.50    1.50   -0.00 (VIOLATED)
_09991_/A1                              1.50    1.50   -0.00 (VIOLATED)
_10376_/B                               1.50    1.50   -0.00 (VIOLATED)
_10377_/A1                              1.50    1.50   -0.00 (VIOLATED)


==========================================================================
cts final max_slew_check_slack
--------------------------------------------------------------------------
-0.00615663081407547

==========================================================================
cts final max_slew_check_limit
--------------------------------------------------------------------------
1.5

==========================================================================
cts final max_slew_check_slack_limit
--------------------------------------------------------------------------
-0.0041

==========================================================================
cts final max_fanout_check_slack
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
cts final max_fanout_check_limit
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
cts final max_capacitance_check_slack
--------------------------------------------------------------------------
0.0029456147458404303

==========================================================================
cts final max_capacitance_check_limit
--------------------------------------------------------------------------
0.5346779823303223

==========================================================================
cts final max_capacitance_check_slack_limit
--------------------------------------------------------------------------
0.0055

==========================================================================
cts final max_slew_violation_count
--------------------------------------------------------------------------
max slew violation count 57

==========================================================================
cts final max_fanout_violation_count
--------------------------------------------------------------------------
max fanout violation count 0

==========================================================================
cts final max_cap_violation_count
--------------------------------------------------------------------------
max cap violation count 0

==========================================================================
cts final setup_violation_count
--------------------------------------------------------------------------
setup violation count 0

==========================================================================
cts final hold_violation_count
--------------------------------------------------------------------------
hold violation count 0

==========================================================================
cts final report_checks -path_delay max reg to reg
--------------------------------------------------------------------------
Startpoint: core.CPU_valid_load_a5$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_Xreg_value_a4[5][0]$_SDFFE_PP1P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock source latency
   0.00    0.00 ^ pll/CLK (avsdpll)
   0.38    0.38 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.33    0.71 ^ clkbuf_4_7_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.19    0.90 ^ clkbuf_leaf_54_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.00    0.90 ^ core.CPU_valid_load_a5$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_2)
   0.42    1.33 ^ core.CPU_valid_load_a5$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_2)
   1.15    2.47 ^ _07947_/X (sky130_fd_sc_hd__or4_4)
   0.81    3.28 v _07949_/Y (sky130_fd_sc_hd__clkinv_16)
   0.73    4.01 v _11035_/X (sky130_fd_sc_hd__or3_2)
   0.27    4.28 v _11036_/X (sky130_fd_sc_hd__o21a_4)
   1.11    5.39 ^ _13430_/Y (sky130_fd_sc_hd__nor3_4)
   0.17    5.56 v _13433_/Y (sky130_fd_sc_hd__a31oi_1)
   0.60    6.16 v hold2286/X (sky130_fd_sc_hd__dlygate4sd3_1)
   0.19    6.35 v _13434_/X (sky130_fd_sc_hd__or2_0)
   0.00    6.35 v core.CPU_Xreg_value_a4[5][0]$_SDFFE_PP1P_/D (sky130_fd_sc_hd__dfxtp_1)
           6.35   data arrival time

  10.70   10.70   clock clk (rise edge)
   0.00   10.70   clock source latency
   0.00   10.70 ^ pll/CLK (avsdpll)
   0.38   11.08 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.34   11.42 ^ clkbuf_4_8_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.19   11.61 ^ clkbuf_leaf_160_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.00   11.61 ^ core.CPU_Xreg_value_a4[5][0]$_SDFFE_PP1P_/CLK (sky130_fd_sc_hd__dfxtp_1)
  -0.54   11.07   clock uncertainty
   0.00   11.07   clock reconvergence pessimism
  -0.11   10.96   library setup time
          10.96   data required time
---------------------------------------------------------
          10.96   data required time
          -6.35   data arrival time
---------------------------------------------------------
           4.62   slack (MET)



==========================================================================
cts final report_checks -path_delay min reg to reg
--------------------------------------------------------------------------
Startpoint: core.CPU_dmem_rd_data_a5[26]$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_Xreg_value_a4[13][26]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock source latency
   0.00    0.00 ^ pll/CLK (avsdpll)
   0.38    0.38 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.29    0.67 ^ clkbuf_4_3_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.17    0.84 ^ clkbuf_leaf_7_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.00    0.84 ^ core.CPU_dmem_rd_data_a5[26]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_2)
   0.38    1.22 v core.CPU_dmem_rd_data_a5[26]$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_2)
   0.32    1.54 v _10561_/X (sky130_fd_sc_hd__and3_4)
   0.10    1.64 ^ _11102_/Y (sky130_fd_sc_hd__nand2_1)
   0.07    1.71 v _11103_/Y (sky130_fd_sc_hd__o211ai_1)
   0.00    1.71 v core.CPU_Xreg_value_a4[13][26]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
           1.71   data arrival time

   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock source latency
   0.00    0.00 ^ pll/CLK (avsdpll)
   0.38    0.38 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.34    0.71 ^ clkbuf_4_11_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.19    0.91 ^ clkbuf_leaf_122_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.00    0.91 ^ core.CPU_Xreg_value_a4[13][26]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.86    1.76   clock uncertainty
   0.00    1.76   clock reconvergence pessimism
  -0.05    1.71   library hold time
           1.71   data required time
---------------------------------------------------------
           1.71   data required time
          -1.71   data arrival time
---------------------------------------------------------
           0.00   slack (MET)



==========================================================================
cts final critical path target clock latency max path
--------------------------------------------------------------------------
0

==========================================================================
cts final critical path target clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
cts final critical path source clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
cts final critical path delay
--------------------------------------------------------------------------
6.3467

==========================================================================
cts final critical path slack
--------------------------------------------------------------------------
4.6178

==========================================================================
cts final slack div critical path delay
--------------------------------------------------------------------------
72.759072

==========================================================================
cts final report_power
--------------------------------------------------------------------------
Group                  Internal  Switching    Leakage      Total
                          Power      Power      Power      Power (Watts)
----------------------------------------------------------------
Sequential             7.05e-03   7.11e-04   1.45e-08   7.76e-03  39.5%
Combinational          1.56e-03   3.42e-03   2.88e-08   4.98e-03  25.3%
Clock                  3.98e-03   2.92e-03   3.21e-09   6.91e-03  35.2%
Macro                  0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
Pad                    0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
----------------------------------------------------------------
Total                  1.26e-02   7.05e-03   4.65e-08   1.96e-02 100.0%
                          64.1%      35.9%       0.0%

```

### Route

```c
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk route
```

![image](https://github.com/user-attachments/assets/c5900d44-3cdc-453c-9aaa-342fe1810109)

![image](https://github.com/user-attachments/assets/2c9589d7-a4a6-4e02-bd4a-bb84c4fbf729)






</details>

</details>
