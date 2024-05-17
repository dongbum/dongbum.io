---
title: auto_ptr, shared_ptr, scoped_ptr
date: 2013-01-18T16:08:49+09:00
categories:
  - C/C++
tags:
  - auto_ptr
  - C
  - pointer
  - scoped_ptr
  - shared_ptr
  - 스마트포인터
  - 포인터
  - 프로그래밍
---

# 스마트포인터

메모리를 사용하는 과정에서 할당이후 해제가 되지 않으면 해당 메모리 공간에는 다른 데이터를 넣을 수 없게 되어 사용가능한 메모리공간이 점점 줄어드는 메모리누수(leak)현상이 발생한다. 또한 해당 메모리 공간에 접근하거나 하여 쓰레기값이 나오거나 혹은 프로그램이 오류를 일으킬 수도 있다. 따라서 할당(malloc, new 등) 이후에는 반드시 해제(free, delete 등)을 해줘야하는데 이 과정의 복잡함과 프로그래밍에서의 실수를 방지하기 위해 자동으로 메모리를 해제해주는 포인터가 바로 스마트포인터이다.

---

## 1. auto_ptr

C++ STL에 있는 기본 스마트포인터. 객체가 특정 스코프(...{....}...)가 종료 될 때 소멸자가 호출된다는 원리를 이용한다.

#### 기본적인 사용법

```cpp
void main()
{
        auto_ptr<int> pInt(new int);
        *pInt = 10;
        cout << *pInt << endl;
}
```

#### auto_ptr의 단점

  1. 스코프 종료 시점에 메모리를 해제하기 때문에 메모리 할당을 배열 단위로 받을 때에는 제대로 메모리를 해제하지 못할 수 있다.
  2. auto_ptr로 할당한 포인터 변수를 서로 대입할 시 (ex : a = b) a와 b가 같은 곳을 가리키는 것이 아니라 a는 b가 가리키고 있던 주소가 들어가고 b는 NULL 상태가 된다. 이렇게 하는 이유는 a와 b가 같은 메모리를 가리키고 있다면 auto_ptr에 의하여 같은 메모리를 두번 해제하려 할 수 있기 때문이다. 따라서, auto_ptr은 같은 메모리 영역을 가리키는 auto_ptr을 2개 이상 생성할 수 없다.
  3. 위와 같은 문제점 때문에 STL의 컨테이너들에는 사용할 수 없다.

그래서 auto_ptr은 C++ 11 에서 deprecated 되었고 앞으로를 위해서라도 이것을 사용하면 안된다.

---

## 2. shared_ptr

auto_ptr의 문제점 때문에 boost에서는 shared_ptr이 생겨나게 되었다. 메모리 포인터가 가리키고 있는 객체의 수를 '레퍼런스 카운터(참조횟수)'라는 것을 이용하여 0에서부터 1씩 증가시키고,  메모리 해제가 될때 1씩 감소시키다가 0이 되면 메모리를 해제한다. 따라서 같은 메모리 영역을 가리키지 못한다는 단점을 극복하게 된다.

#### 기본적인 사용법

```cpp
#include <iostream>
#include <vector>
#include <boost/shared_ptr.hpp>

using namespace std;
using namespace boost;

class ClassA
{
public:
	ClassA(){cout << "ClassA()" << endl;}
	~ClassA(){cout << "~ClassA()" << endl;}
	void hello(){cout << "# hello()" << endl;}
};

vector< shared_ptr<ClassA> > vec;

int main () {
	shared_ptr<ClassA> obj(new ClassA());
	cout << "* use_count (new): " << obj.use_count() << endl;

	vec.push_back(obj);
	cout << "* use_count (push): " << obj.use_count() << endl;

	obj->hello();

	vec.pop_back();
	cout << "* use_count (pop): " << obj.use_count() << endl;

	obj.reset(new ClassA());
	cout << "* use_count (reset): " << obj.use_count() << endl;

  return 0;
}
```

shared_ptr로 할당된 객체는 use_count()라는 함수를 통해 레퍼런스 카운터의 상태를 알아낼 수 있다.

####  shared_ptr의 단점

  1. 레퍼런스 카운터를 이용하기 때문에 환형 링크드리스트처럼 head->next->...->next->tail->head 식으로 계속 다음을 가리키는 경우에는 레퍼런스 카운터가 0이 되지 않아 메모리가 해제되지 않을 수 있다.

---

## 3. scoped_ptr

동적으로 할당되는 객체에 대한 포인터를 가진다. 자신이 삭제되면 가리키고 있는 객체도 자동으로 삭제시킨다. 이 포인터는 복사가 불가능하게 되어있으므로 소유권이나 참조카운트 등의 문제가 발생하지 않는다.

#### 기본적인 사용법

```cpp
// ScopedPtrTest.h
#include <iostream>
#include <memory>
#include <boost/scoped_ptr.hpp>

using std::cout;
using std::endl;

class Sample
{
   public:
       Sample() { cout << "생성" << endl; }
       ~Sample() { cout << "소멸" << endl; }
       void print() { cout << "print" << endl; }
};

// ScopedPtrTest.cpp
#include "ScopedPtrTest.h"

typedef boost::scoped_ptr<Sample> scopePtr;

void printStr(scopePtr* Ptr); // 값 복사가 불가능하므로 포인터 형태로 넘겨야 한다.

int main(void)
{
    scopePtr p1(new Sample());
    scopePtr p2(new Sample());

    p1->print();
    printStr(&p1); // 스마트 포인터 scoped_ptr(scopePtr)의 포인터를 넘긴다.

   /*
       // scoped_ptr - 자신이 가지는 포인터를 다른 인스턴스로 할당하거나 넘겨줄 수 없다.
       //                    한번 가리키는 객체에 대한 삭제의 책임을 전적으로 진다.
       //                    (복사가 빈번한 STL에서 사용할 수 없다.)

    p2 = p1             // 에러! 할당 불가!
    scopePtr p3(p1) // 에러!
   */

   return 0;
}

void printStr(scopePtr* Ptr) // 값 복사가 불가능하므로 call by reference(address)로 인자를 넘긴다.
{
    (*Ptr)->print();
}
```

#### scoped_ptr의 단점

  1. 포인터를 다른 변수에 복사하거나 할당할 수 없다.
  2. STL의 컨테이너에 넣을 수 없다.
  3. 동적으로 생성한 객체들의 배열을 만들고 싶을 때에는 scoped_array를 이용한다.

---

### 출처 & 참고URL

  * http://psychoria.blog.me/40155382971
  * http://sweeper.egloos.com/2826435
  * http://bangkert89.blog.me/90161420244
  * http://blog.naver.com/ddongtong18/120161513218
  * http://breaklee.blog.me/60137887986
  * http://sanaigon.tistory.com/72
  * http://codecooking.tistory.com/21
  * http://blog.daum.net/creazier/15309389
  * http://silped.tistory.com/3
  * http://blog.naver.com/jjmyann123?Redirect=Log&logNo=120168606246
  * http://blog.naver.com/xelona?Redirect=Log&logNo=70041650058
