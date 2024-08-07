# ASIC_Design
## Lab 1:
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
 #### 1. Compile and verify using RISC-V GNU compiler and observe difference between Ofast and O1
 -O1 optimization:
 ##### Commands for compiling O1 optimization:
 ```
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o 1tonsum.o 1tonsum.c
```
##### Output image:
![image](https://github.com/user-attachments/assets/43d0d068-4529-4c1c-82be-ca7874942545)

