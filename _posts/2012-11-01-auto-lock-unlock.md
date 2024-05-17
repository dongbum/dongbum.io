---
title: 쓰레드 처리를 위한 Auto Lock/Unlock 클래스
date: 2012-11-01T18:28:49+09:00
categories:
  - C/C++
  - Note of the wrong answers
  - Programming
tags:
  - C
  - Critical Section
  - Lock
  - Thread
  - Unlock
  - 쓰레드
---
오늘 공부의 목표는 Lock/Unlock의 자동화였다.

CRITICAL_SECTION을 사용할 때 이것을 초기화, 삭제해주는 것뿐만 아니라 EnterCriticalSection/LeaveCriticalSection을 일일히 수동으로 해준다는 것은 귀찮을 뿐만 아니라 사람의 실수에 의하여 문제가 생길 소지가 있는 부분이기 때문. 만약 언락을 안하면 데드락 상황에 빠질 수 있기 때문에 반드시 언락을 해줘야하는데 어떤 프로그램 로직상 혹은 작성자의 실수로 이런 경우가 생길 수 있다. 그래서 그걸 아예 자동화해주는게 Auto Lock/Unlock 클래스.

두개의 헤더파일로 구성된다.

lock.h 파일

```cpp
#include <iostream>
#include <Windows.h>
#include <process.h>

class Lock
{
public :
	Lock()
	{
		// std::cout << "INIT" << std::endl;
		InitializeCriticalSection(&cs_);
	}

	~Lock()
	{
		// std::cout << "DELETE" << std::endl;
		DeleteCriticalSection(&cs_);
	}

	void lock()
	{
		// std::cout << "LOCK" << std::endl;
		EnterCriticalSection(&cs_);
	}

	void unlock()
	{
		// std::cout << "UNLOCK" << std::endl;
		LeaveCriticalSection(&cs_);
	}
private :
	CRITICAL_SECTION cs_;
};
```

크리티컬 섹션을 생성하고 초기화와 삭제, 락과 언락을 해주는 메서드로 이루어져있다.

그다음은 AutoLockUnlock.h 파일

```cpp
#include <iostream>

#include "lock.h"

class AutoLockUnlock
{
private :
	static Lock lock_;
public :
	AutoLockUnlock()
	{
		// std::cout << "AutoLockUnlock() 생성자" << std::endl;
		lock_.lock();
	}

	~AutoLockUnlock()
	{
		lock_.unlock();
		//std::cout << "AutoLockUnlock() 소멸자" << std::endl;
	}
protected :
private :
};

Lock AutoLockUnlock::lock_;
```

실제사용은 autolockunlock.h 파일을 인클루드하고 스코프를 만들어주기 위해 락이 필요한 부분을 {, }로 감싼 다음 해당 블록 안 제일 첫줄에 AutoLockUnlock autolockunlock; 처럼 하나 선언해준다. 블록이 시작되면 AutoLockUnlock의 생성자에 의해 크리티컬섹션의 초기화와 락이 시작되고 코드 블록이 종료되면 AutoLockUnlock의 소멸자에 의하여 언락이 되고 크리티컬섹션 삭제까지 된다.

아래와 같은 쓰레드 함수가 있다면 맨처음에 AutoLockUnlock 타입을 같은 변수를 생성함으로써 초기화/락/언락/삭제를 한번에 할 수 있다.

```cpp
unsigned int WINAPI ThreadFunc(void * arg)
{
	AutoLockUnlock autolock; // 여기서 오토락을 건다. 생성만 해주면 알아서 만들어지고 알아서 삭제된다.
	std::cout << "쓰레드 시작 :: " << target << std::endl;

	for (int i = 0; i < 50000; i++)
	{
		target = target + 1;
	}

	for (int i = 50000; i < 100000; i++)
	{
		target = target - 1;
	}
	std::cout << "쓰레드 종료 :: " << target << std::endl;
	return 0;
}
```

중요한건 {, }에 의한 코드블록에 의해서 AutoLockUnlock 클래스의 생성자와 소멸자가 호출된다는 점. 이것을 이용하여 자동화된 Lock/Unlock을 구현한다.

쓰레드를 쓸 때는 항상 이러한 자동화된 락과 언락을 사용함으로써 데드락에 빠지는 사태를 막을 수 있다. 쓰레드를 쓸 때는 무조건 사용할 것.
