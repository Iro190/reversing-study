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
