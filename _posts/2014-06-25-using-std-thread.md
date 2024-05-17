---
title: std::thread 사용법
date: 2014-06-25T22:03:15+09:00
categories:
  - C/C++
tags:
  - C
  - Thread
  - 스레드
  - 쓰레드
---
간단히 스레드를 만들어 테스트 해야할 일이 있어 `std::thread`를 찾아보고 만들어봤다.

예전에는 `boost::thread`를 이용했었는데 C++11에 오면서 아예 다 내장되었다는게 놀랍고 신기하다.

```cpp
#include <iostream>
#include <thread>
#include <windows.h>

char* target_msg = "test";
char msg[1024] = {0,};

void callback_function(int thread_num)
{
    while(true)
    {
        strcpy_s(msg, target_msg);
        //std::cout << "copy (" << thread_num << ")" << std::endl;
        Sleep(10);
    }
}

int main(void)
{
    std::thread thread1(callback_function, 1);
    std::thread thread2(callback_function, 2);

    thread1.join();
    thread2.join();

    return 0;
}
```
