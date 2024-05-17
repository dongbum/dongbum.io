---
title: InitializeCriticalSectionAndSpinCount, CriticalSection에 대한 정보 이것저것
date: 2017-12-04T11:16:27+09:00
categories:
  - C/C++
---
개인적으로 만들고 있는 프로젝트, 프로그램들은 대부분 리눅스/윈도우간의 크로스플랫폼을 지향하고 있는데 그래서 그런지 락을 쓸 때는 std::mutex를 자주 쓰고 있다.

최근에는 윈도우의 critical section이 더 빠르다는 얘기가 자꾸 들려서 윈도우일 때는 critical section을 더 쓰도록 변경하는 중.

그러다가 **InitializeCriticalSection** 이라는 초기화함수 대신 **InitializeCriticalSectionAndSpinCount** 라는 함수를 보게 되었는데 이게 무엇인지 찾아보았다. 이 함수의 인자는 2개인데 첫번쨰는 당연히 크리티컬섹션 객체의 포인터이고 뒤에 붙는 인자는 스핀 횟수이다.

어떤 스레드가 작업을 하다가 크리티컬섹션에 걸려서 InitializeCriticalSection 을 썼다면 대기상태에 들어가게 된다. InitializeCriticalSectionAndSpinCount 를 썼다면 바로 대기상태에 들어가지 않고 인자로 줬던 SpinCount 횟수만큼 기다리게 된다. 만약 그 사이에 크리티컬섹션이 풀리면 바로 다시 작업 진행. SpinCount 횟수만큼 기다렸는데도 크리티컬섹션이 풀리지 않았다면 대기상태로 들어간다.

InitializeCriticalSectionAndSpinCount는 멀티프로세서 시스템에서만 유효하며 싱글프로세서 시스템에서는 뒤에  SpinCount 값은 무시되고 0으로 설정된다. 멀티프로세서 시스템인 경우, 크리티컬섹션에 걸려 대기에 들어가지 않으므로써 스레드간 Context Switching이 적어지므로 결과적으로 InitializeCriticalSection 을 사용할 때보다 성능이 더 나아질 수 있다.

또한 InitializeCriticalSection 의 리턴은 void 이기 때문에 크리티컬섹션 초기화가 제대로 되었는지 알 수가 없다. 메모리 부족 상황 등에서 초기화 실패가 일어날 수 있고 이 경우 InitializeCriticalSection 함수 부분에서 exception이 일어나게 되어 추적하기 힘든 버그가 발생할 수 있다. InitializeCriticalSectionAndSpinCount 함수는 리턴값이 있고 이러한 버그 상황을 피할 수 있도록 만들어준다.

```cpp
// 예제 코드
CRITICAL_SECTION g_cs;
int main()
{
    if (!InitializeCriticalSectionAndSpinCount(&g_cs, 4000))
    {
        // 예외 처리
    }
    ...
}
// [출처] [VC++] 크리티컬 섹션 초기화 관련 간단 팁|작성자 데브머신</pre>
```

이와 관련하여 스핀카운트의 단위가 무엇인지, 스레드 컨텍스트 스위칭이 이루어지지 않고 크리티컬섹션이 풀릴 때까지 어디서 어떻게 기다리는지 더 알아보고 싶지만 구글을 뒤져봐도 자료가 없는듯하다.

<참고자료>

  * http://blog.naver.com/bastard9/140049467862
  * http://girtowin.tistory.com/107
  * http://devmachine.blog.me/220151128032
  * http://blog.naver.com/ccw3435/100055320847
  * https://msdn.microsoft.com/en-us/library/ms683476(VS.85).aspx
