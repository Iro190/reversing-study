# reversing-study

**어셈블리 코드 역분석 및 c언어로 복원하기!**

## 01. 어셈블리 기초 연산 분석

### 1. 어셈블리 코드(Assembly)
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
**질문 1.**
  ```check_password 함수를 통과하여 성공(eax = 1)하려면, 처음에 입력해야 하는 숫자 X는 10진수로 얼마일까요? ```
**질문 2.**
  ```이 어셈블리 로직을 C 언어의 if 조건문 형태로 간단히 나타낸다면 어떤 식일까요?```

### 02. 역분석 과정
  mov eax, edi: 입력값(edi)을 eax에 복사
  add eax, 0x10: eax + 16(10진수)
  sub eax, 0x5: eax - 5(10진수)
  cmp eax, 0x20: 연산 결과가 32(10진수)인지 비교
  je  success: 맞으면 성공

  수식:x + 16 - 5 = 32
  역계산:x = 32 + 5 - 16
                     = 21

### 03. c언어로 복원
## 첫 번째
    int main() {
      int x;
      scanf("%d", &x);
      x + 16 - 5;

      if(x==32)
        printf("성공");
      else
        printf("실패");S

      return 0;

    }

## 두 번째(피드백) 
    int check_password(int x) {
      int result = x + 16 - 5;  // 1~3번줄: eax = x + 16 - 5

      // 4~7번줄: cmp result, 32 / je success
      if(result == 32) {
        return 1;  // 성공 (mov eax, 1)
      } else {
        return 0;  //실패 (mov eax, 0)
        }
    }
  
