---
title: 'C++에서 #변수명 사용법'
date: 2013-02-19T11:33:43+09:00
categories:
  - C/C++
tags:
  - C
  - 변수
  - 변수명
---
C++에서 가끔 #을 붙여쓰는 변수를 봤는데 오늘 용도를 알았다.

```cpp
#include <iostream>

#define Test(a) { printf("%s", #a); }

int main(void)
{
  int aaa = 1000;
  Test(aaa);

  return 0;
}
```

이렇게 코드를 입력하고 실행하면 Test() 안의 #a는 aaa 값인 1000이 찍히는게 아니라 Test(a)의 인자로 들어온 변수명인 aaa의 값이 찍힌다.

LOG4CXX를 사용할 때 클래스의 이름을 태그로 출력하거나 할 때 자주사용되므로 기억해놓을 것.
