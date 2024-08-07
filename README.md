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
There are a total of 11 lines in the main section of O1 optimization.
![image](https://github.com/user-attachments/assets/63b63e54-3d8d-4b04-a695-ce6e43cc1e8d)

</details>

<details><summary><strong>Lab2</strong></summary>


</details>

