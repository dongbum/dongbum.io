---
title: 주민등록번호 검증 코드
date: 2012-06-10T00:19:28+09:00
categories:
  - C/C++
tags:
  - C
  - 주민등록번호
---
C/C++에서 주민등록번호를 검증하는 코드이다.

아래의 함수는 숫자 배열을 가지고 작업하도록 되어있지만 원하는 바에 따라 문자열도 처리가능하다.

```cpp
BOOL VerifyJuminNumber(int number[]) {
  const int tab[] = {2,3,4,5,6,7,8,9,2,3,4,5};
  int sum = 0;
  int i = 0;
  for(i=0 ; i < 12 ; i++)
  sum += number[i] * tab[i];
  return ((11 - (sum % 11)) % 10 == number[12]) ? TRUE : FALSE;
}
```
