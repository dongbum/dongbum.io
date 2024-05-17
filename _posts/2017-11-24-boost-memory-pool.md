---
title: boost에서 메모리풀 사용
date: 2017-11-24T09:56:32+09:00
categories:
  - boost
  - C/C++
tags:
  - boost
  - 메모리풀
---
프로그램 구현 중 메모리풀이 필요한 경우가 있어 어떻게 할까 하다가 boost의 메모리풀을 찾아보게 되었다.

boost의 메모리풀은 4가지.

  1. pool
  2. singleton_pool
  3. pool_alloc
  4. object_pool

네가지 메모리풀 중에 어떤 것이 어떤 상황에 가장 적합한지는 많이 써봐야 알 수 있을듯하다. 찾아보니 내가 가지고 있던 몇몇 게임서버 소스는 singleton_pool을 사용하고 있는 케이스가 있었다. 이것부터 먼저 찾아보는 것으로.

singleton_pool은 다른 메모리풀 클래스들과는 다르게 스레드-세이프 특성을 지니고 있어서 가장 유용하게 쓰일 것 같다.

singleton_pool을 아래와 같은 코드로 좀더 편하게 사용 가능. 출처는 '아지크의 좌충우돌 IT 이야기'

```cpp
#include <stdio.h>
#include <iostream>
#include <vector>
#include <boost/pool/singleton_pool.hpp>
#include <boost/pool/pool_alloc.hpp>

template<typename T,  unsigned int NEXT_SIZE = 32U, unsigned int  MAX_POOL_SIZE = 0U>
class CMemoryPoolT
{
public:
     static void* operator new(size_t size)
     {
          return boost::singleton_pool<T,
               sizeof(T),
               boost::default_user_allocator_new_delete,
               boost::details::pool::default_mutex,
               NEXT_SIZE,
               MAX_POOL_SIZE>::malloc();
     }

     static void operator delete(void* p)
     {
          boost::singleton_pool<T,
               sizeof(T),
               boost::default_user_allocator_new_delete,
               boost::details::pool::default_mutex,
               NEXT_SIZE,
               MAX_POOL_SIZE>::free(p);
     }
};


class CMemoryPoolTest  :public CMemoryPoolT<CMemoryPoolTest>
{
public:
     char Dumy[124] ;
};


int _tmain(int argc, _TCHAR* argv[])
{
     using std::vector ;

     CMemoryPoolTest *p = new CMemoryPoolTest() ;

     delete p ;

     printf("Using MemoryPool... \n") ;

     return 0;
}
```

프로그램 종료시에는 purge_memory()를 꼭 호출해주어야 한다고 한다.

혹은 다음과 같은 방법으로도 사용 가능하다.

```cpp
struct SEND_BUFFER_TAG {};
typedef boost::singleton_pool<SEND_BUFFER_TAG, SEND_BUFFER_SIZE> SendBufferPool;
```

사용할 클래스의 헤더파일에서 위와 같이 선언. TAG를 여러개 사용하고 메모리풀마다 다르게 적용하면 서로 다른 메모리풀이 된다.

메모리를 할당 받고 싶다면,

```cpp
char* send_data = static_cast<char*>(SendBufferPool::malloc()); // 메모리를 할당 받는다.

SendBufferPool::free(send_data); // 다 사용한 메모리를 해제한다.
```

종료시에는 당연히,

```cpp
SendBufferPool::purge_memory();
```

를 호출해야한다.

출처와 참고사이트

  * <http://postgame.tistory.com/32>
  * <http://blog.naver.com/cmw1728/220663109747>
  * <http://blog.naver.com/sorkelf/40135434878>
  * <http://copynull.tistory.com/164>
  * <http://azeke.tistory.com/entry/Boost%EC%9D%98-MemoryPool-%ED%8E%B8%ED%95%98%EA%B2%8C-%EC%9E%91%EC%84%B1%EC%9D%84-%ED%95%98%EC%9E%90>
