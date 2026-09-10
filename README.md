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
    
## ◼️Assembly basic operation analysis_4 
  
### Assembly code
```assembly
check_value:
  push rbp
  mov rbp, rsp

  mov eax, edi
  cmp eax, 0x0A
  jle is_small

  add ecx, 0x05
  jmp cleanup

is_small:
  sub ecx, 2

cleanup:
  pop rbp
  ret
```
**Q1.**
  ```Will the returned eax value be different when the decimal number 15 is entered as the input value edi to the check_value function? ```  
  
**Q2.**
  ```Conversely, what is the returned value of eax when the decimal number 7 is entered as the input value edi?```  

**Q3.**
  ```Try to reconstruct the entire logic of this assembly function into the form of the C language function int check_value(int x)```

### 01. Reverse calculation process
  mov eax, edi - Copy the first parameter (x) to eax  
    cmp eax, 0x0A - Compare eax with decimal 10 (0x0A)  
    jle is_small - If eax is 10 or less (Less or Equal), jump to is_small  
    add eax, 0x05 - If greater than 10, eax + 5  
    jmp cleanup - Go unconditionally to the cleanup point  
    
is_small:  
    sub eax, 0x02 - If 10 or less, eax - 2  
    
formula: 15 + 5 = 20 (Q1) 15>10  
formula: 7 - 2 = 5 (Q2) 7<10

### 02. Restore to C
## Frist  
**In the case of the 15th**  
```
int check_value(int x);  

int main() {  
  int result = check_value(15);  
  return 0;  
}  

int check_value(int x) {  
    if (x <= 10)  
    {  
       printf("%d", x - 2);  
    }  
    else{  
        printf("%d", x + 5);  
        }  
        
    return 0;  
}
```

**In case of 7**  
```int check_value(int x);

int main() {
    int result = check_value(7);
    return 0;
}

int check_value(int x) {
    if (x <= 10)
    {
       printf("%d", x - 2);
    }
    else{
        printf("%d", x + 5);
    }
    
    return 0;
}
```

## Second(feedback)
  ```The eax value calculated in assembly is used as the function's return value.```  
  
  ```While printing with printf is a very good way to verify the operation, to match the principle of returning the eax value at the moment of ret in assembly, writing it in a form that immediately returns the calculated result as shown below will result in a more accurate match with the code restored by the decompiler.```  
  
```int check_value(int x) {
    if (x <= 10) {
        return x - 2;  
    } else {
        return x + 5;  
    }
}

int main() {  
    printf("15 결과: %d\n", check_value(15)); // 20  
    printf("7 결과: %d\n", check_value(7));   // 5  
    return 0;  
}
```
## Third (different method)
**I tried changing it to read data inside the function using scanf**  

```int check_value(int x) {
    scanf("%d", &x);

    if (x <= 10)
    {
        return x - 2;
    } else {
        return x + 5;
     }
   
  return 0;  
}  
```
**Modify code after feedback**  
```int check_value() {  
    int x;
    scanf("%d", &x);

    if (x <= 10)
    {
        return x - 2;
    } else {
        return x + 5;
     }
   
}
```

## ◼️Assembly basic operation analysis_5 
  
### Assembly code
```assembly
; rdi: starting address of array (int arr[])
; esi: index number of array (int index)

get_element:
    push rbp
    mov rbp, rsp

    movsxd rax, esi
    mov eax, [rdi + rax*4]
    add eax, 10

    pop rbp
    ret 
```
**Q1.**
  ```Given the C array `int arr[3] = {5, 12, 30};`, what is the value of `eax` returned when `get_element(arr, 1)` is called? (Array indices start from 0.) ```  
  
**Q2.**
  ```Try to restore the entire assembly function into the form of the C language function int get_element(int arr[], int index).```  

### 01. Reverse calculation process
  get_element:  
    push rbp  
    mov rbp, rsp  
    movsxd rax, esi - Extend index (esi) to 64-bit (rax)  
    mov eax, [rdi + rax*4] - Get array elements  
    add eax, 10 - Add 10 to the retrieved value  
    pop rbp  
    ret  
    
formula: 12 + 10 = 22 (Q1)

### 02. Restore to C
## Frist    
```
int get_element(int arr[], int index) {
    arr[5, 12, 30];

    int result = arr[1] + 10 ;

    printf("%d", result);
    return 0;
}
```

## Second(feedback)
  ```1. Direct declaration of array values ​​is only possible within the main function.```  
  
  ```2. To use the passed index like the assembly [rdi + rax*4] syntax, you must write arr[index].```  
  
```
int get_element(int arr[], int index) {
    // mov eax, [rdi + rax*4] -> Accessing the arr[index] element
    // add eax, 10           -> 10 plus
    return arr[index] + 10;
}

int main() {
    int arr[3] = {5, 12, 30};

    // When call get_element(arr, 1), index 1 (12) + 10 = 22 is returned.
    int result = get_element(arr, 1);

    printf("resulte: %d\n", result); // 22
    return 0;
}
```


## ◼️Assembly basic operation analysis_6 
  
### Assembly code
```assembly
; rdi: starting address of array (int arr[])
; esi: index number of array (int index)
; edx: New value to save (int value)

update_element:
    push rbp
    mov rbp, rsp
    movsxd rax, esi
    mov [rdi + rax*4], edx
    mov eax, [rdi + rax*4]
    add eax, 5
    pop rbp
    ret
```
**Q1.**
  ```Given the C array `int arr[3] = {10, 20, 30};`, what is the final returned value of `eax` when `update_element(arr, 1, 50)` is called? ```  
  
**Q2.**
  ```After the function is executed, what will the value remaining in arr[1] be?```  

**03.**  
```Try restoring the entire assembly function into the form of the C language function int update_element(int arr[], int index, int value).```

### 01. Reverse calculation process
  update_element:  
    push rbp  
    mov rbp, rsp  
    movsxd rax, esi - Extend index (esi) to 64 bits  
    mov [rdi + rax*4], edx - Store the edx value at the corresponding position in the array  
    mov eax, [rdi + rax*4] - Read the array element just modified into eax  
    add eax, 5 - Add 5 to eax      
    pop rbp  
    ret 
    
update_element(arr, 1, 50)  
first argument(arr) -> rdi(starting address of array)  
second argument(1) -> esi(Array index number = position)  
third argument(50) -> edx(new value to change)  
arr[1] = 20 -> arr[1] = 50  

formula: 50 + 5 = 55 (Q1)  

formula: arr[1] = 20 -> arr[1] = 50  
                 = 50 (Q2)  

### 02. Restore to C
## Frist    
```
int update_element(int arr[], int index, int valu) {
    arr[1] = 50;
    return arr[index] +5;
}

int main() {
    int arr[3] ={10, 20, 30};

    int result = update_element(arr, 1, 50);
    printf("%d\n", result);

    return 0;   
}
```

## Second(feedback)
  ```Even if you input a fixed value like `arr[1] = 50;`, 55 is output correctly in the current test (index 1, value 50); however, if you pass a different index or value, such as `update_element(arr, 2, 100)`, the behavior changes.```  
  
  ```If you modify the assembly code to use variable parameters (index, value) while keeping the `[rdi + rax*4]` and `edx` parameters intact, it becomes a perfect function capable of handling any input value.```  
  
```
int update_element(int arr[], int index, int valu) {
    arr[index] = value;  //Assigning values ​​to array elements
    return arr[index] +5;
}

// eax = arr[index]; (Reading the value of an array element)

int main() {
    int arr[3] ={10, 20, 30};

    int result = update_element(arr, 1, 50);
    printf("%d\n", result);

    return 0;   
}
```
