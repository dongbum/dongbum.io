---
title: cin을 쓸 때 입력을 한번 지나가는 현상을 해결하는 방법
date: 2012-10-30T20:30:34+09:00
categories:
  - C/C++
  - Note of the wrong answers
  - Programming
tags:
  - C
  - cin
  - getline
  - 문자열
  - 입력
---
오늘 공부하던 중 발견한 부분.

cin을 통해 문자 하나를 입력 받은 후 getline으로 다른 문자열을 입력 받으려고 할 때 문제가 생긴다. 입력을 한번 건너뛰는 현상.

cin으로 입력 받은 다음 다른 이벤트(?)가 더 오는 것 때문이라고 한다. 이 문제를 해결하기 위해서 **std::fflush(stdin);** 을 이용하면 이 현상이 사라진다. cin을 이용할 때는 반드시 기억해놔야할듯.

```cpp
std::string temp;
std::cin >> temp;
std::cout << "start" << std::endl;
int rtn = std::fflush(stdin);

std::string msg;
std::getline(std::cin, msg);
std::cout << "end" << std::endl;
```
