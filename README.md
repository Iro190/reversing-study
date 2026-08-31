# reversing-study

**Reverse Assembly Code and Restore to C**

## ◼️Assembly basic operation analysis_1

### Assembly code
```assembly
check_password:
  push rbp
  mov rbp, rsp
  mov eax, edi
  add eax, 0x10
  sub eax, 0x5
  cnp eax, 0x20
  je  success
  mov eax, 0
  pop rbp
  ret
```
**Q1.**
  ```What is the decimal value of the first number X that must be entered to pass the check_password function successfully (eax = 1)? ```  
  
  **Q2.**
  ```If we were to simply represent this assembly logic in the form of a C if conditional statement, what would it look like?```

### 01. Reverse calculation process
  mov eax, edi - Copy input value (edi) to eax 
  add eax, 0x10 - eax + 16(Decimal)  
  sub eax, 0x5 - eax - 5(Decimal)  
  cmp eax, 0x20 - Compare whether the operation result is 32 (decimal) 
  je  success - If correct, success

  formula:x + 16 - 5 = 32  
    Back calculation:x = 32 + 5 - 16
                                 = 21

### 02. Restore to C
## Frist
    int main() {
      int x;
      scanf("%d", &x);
      x + 16 - 5;

      if(x==32)
        printf("success");
      else
        printf("fale");S

      return 0;

    }

## Second(feedback) 
    int check_password(int x) {
      int result = x + 16 - 5;  // 1L~3L: eax = x + 16 - 5

      // 4L~7L: cmp result, 32 / je success
      if(result == 32) {
        return 1;  // success (mov eax, 1)
      } else {
        return 0;  //fale (mov eax, 0)
        }
    }  
    

## ◼️Assembly basic operation analysis_2 
  
### Assembly code
```assembly
  push rbp
  mov rbp, rsp
  mov eax, 0x10
  add eax, 0x05
  sub eax, 0x03
  mov eax, 0
  pop rbp
  ret
```
**Q1.**
  ```What will be the final decimal value stored in eax? ```  
**Q2.**
  ```If I convert this into C language code, how can I use it?```

### 01. Reverse calculation process
  mov eax, 0x10 - Copy 16 (Decimal) to eax  
  add eax, 0x05 - eax + 5(Decimal)  
  sub eax, 0x03 - eax - 3(Decimal)

formula: 16 + 5 - 3 = 18  
               eax = 18

### 02. Restore to C
## Frist
    int main() {
      int x = 16;
      printf("%d + 5 - 3", x);

      return 0;
    }

## Second(feedback)
    int main() {
      int x = 16;
      printf("%d\n", x + 5 - 3); //Fix wrong code

      return 0;
    }

## Third (different method)
    int main() {
      int x = 16;
      x = x + 5 - 3;
      printf("%d\n", x);

      return 0;
    }

  
## ◼️Assembly basic operation analysis_3 
  
### Assembly code
```assembly
  push rbp
  mov rbp, rsp
  mov eax, 0
  add ecx, 5

  loop_start:
  add eax, ecx
  sub ecx, 1
  cmp ecx, 0
  jg loop_start

  mov edx, eax
  pop rbp
  ret
```
**Q1.**
  ```What are the final decimal values ​​stored in the eax and edx registers after this assembly code is fully executed? ```  
  
**Q2.**
  ```Try to reconstruct the core logic of this code (what calculations it performs) in C. (You can use whichever is more comfortable, a for loop or a while loop.)```

### 01. Reverse calculation process
  mov eax, 0 - Copy 0 to eax  
  mov ecx, 5 - Copy 5 to ecx  
  
  loop_start:  
  add eax, ecx - Add eax and ecx  
  sub ecx, 1 - Sub 1 from ecx  
  cmp ecx, 0 - Cmp ecx(4) and 0  
  jg loop_start - Since ecx(4) is greater than 0, jump back to loop_start  
  
  mov edx, eax - Copy edx to eax

answer_1 (worng)  
formula: 0 + 5 - 1 = 4  
= Since the line mov edx, eax is outside the loop, the value of eax is 0 and the value of ecx is 5  

answer_2 (Correct)  
formula:  
1) eax = 0 + 5 = 5, ecx = 4  
2) eax = 5 + 4 = 9, ecx = 3  
3) eax = 9 + 3 = 12, ecx = 2  
4) eax = 12 + 2 = 14, ecx = 1  
5) eax = 14 + 1 = 15,ecx = Execute 'mov edx', eax after loop ends  

= It is true that 'mov edx, eax' is outside the loop, but after the loop ends, eax is not 0 but the sum of all values ​​from 5 to 1. So the final answer is eax = 15, edx = 15.
               

### 02. Restore to C
## Frist
    int main() {
      int a = 0;
      int b = 5;

      while (b > 0)
      {
          a + b - 1;
          break;
      } 
    
      int c = a;
      printf("eax = %d, ecx = %d", a, b);
        
      return 0;
    }

## Second(feedback)
  ```Just catch the continuously repeating flow without the += assignment operator and break.```  
  
    int main() {
      int eax = 0;
      int ecx = 5;

      // Repeat while ecx is greater than 0
      while (ecx > 0) {
          eax += ecx; // add eax, ecx
          ecx--;      // sub ecx, 1
        }

      int edx = eax;  // mov edx, eax

      printf("eax = %d, edx = %d, ecx = %d\n", eax, edx, ecx);
    
      return 0;
    }

## Third (different method)
**Convert to for statement**  

    for (int i = b; i > 0; i--)
    {
        a + b - 1;
    }

**Convert to for statement_2 (feedback)**  

```1. a + b - 1; Adding an assignment operator to the expression```  
```If you write only a + b - 1;, the CPU performs the calculation but discards the result without storing it anywhere.```
```In assembly, add eax and ecx store the result in eax -> a += i; or a = a + i;```
```The assembly sub ecx, 1 subtracts 1 from the value of ecx -> the i-- in the for loop performs this role instead.```
  
```2. Since the for loop condition started with 'int i = b;', the decrementing variable 'i' must be added instead of 'b' inside the loop.```
   
    int main() {
      int a = 0;
      int b = 5;

      // It repeats as ecx(b) decreases from 5 to 1
      for (int i = b; i > 0; i--) {
          a += i;  // Performs the role of add eax, ecx and performs cumulative addition
      }

      int c = a;   // mov edx, eax

      printf("eax(a) = %d, edx(c) = %d\n", a, c); // 15, 15
      return 0;
    }
    
