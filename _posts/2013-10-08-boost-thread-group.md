---
title: boost::thread_group
date: 2013-10-08T11:47:32+09:00
categories:
  - C/C++
tags:
  - boost
  - Thread
---
부스트의 쓰레드 그룹을 사용하기 위한 코드

출처 : <http://nerv2000.tistory.com/76>

```cpp
#include "boost/thread.hpp"

class threadFunc
{
    int m_a;
public:
    threadFunc( int a )
    {
        m_a = a;
    }

    void operator()()
    {
        printf("[%d]일단 들어왔네!!! [%d]n", boost::this_thread::get_id(), m_a );

        Sleep(5000);

        printf("[%d] 끝났네 [%d]n", boost::this_thread::get_id(), m_a );
    }
};

int main()
{
    boost::thread_group tg;

    tg.create_thread( threadFunc(1) );                        // 1번 스래드 생성
    tg.add_thread(new boost::thread( threadFunc(2) ) );        // 2번 스래드 생성
    tg.add_thread(new boost::thread( threadFunc(3) ) );        // 3번 스래드 생성

    //모든 스래드가 종료 될때까지 대기
    tg.join_all();

    return 0;
}
```
