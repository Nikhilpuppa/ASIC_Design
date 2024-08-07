# ASIC_Design
## Lab 1:
### Objective:
  - Compile and verify a simple C code (example: sum of n natural numbers) in both GCC and RISC-V GNU compiler tool chain on Ubuntu.
### Tools used:
  - GCC
  - RISC-V GNU complier.
### Methodolgy:
  #### Task_1 : Compile and verify simple C code in GCC environment.
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

  
