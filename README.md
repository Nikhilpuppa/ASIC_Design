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
