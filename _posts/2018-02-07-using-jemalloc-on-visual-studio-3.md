---
title: Visual Studio에서 jemalloc 설치하고 사용법 (3)
date: 2018-02-07T15:49:12+09:00
categories:
  - C/C++
tags:
  - C
  - jemalloc
  - Memory Pool
  - Visual Studio
  - 메모리풀
---
jemalloc 라이브러리가 정상 작동하므로 솔루션에 포함되어 있는 test_threads 프로젝트를 빌드해본다.

별 설정 없이도 잘 빌드되고 실행된다.

....만, 이게 도대체 뭔 내용인지는 나도 잘 모르겠다. test\_threads.cpp 파일에는 테스트용 코드가 잔뜩 있다. 다 읽어보기도 힘들어서 읽어보다가 관둿다. 대략 je_malloc, je_free 등의 함수를 이용하는 예제코드인 것 같다.

github에는 간단한 예제코드가 있다. (<https://github.com/jemalloc/jemalloc/wiki/Getting-Started>)

....는 별로 도움이 되지 않았다.

그냥 쉽게 얘기하면 new/malloc 을 할 때 je_malloc 을 하면 되고 delete/free 를 할 때 je_free 를 하라면 되는 말.

그래서 new와 delete를 오버라이딩하는 클래스를 만든다.

new 를 사용하는 곳에서 이 오버라이딩 클래스를 상속 받게 해주면 끝.

그리고 헤더에서 #include `<jemalloc/jemalloc.h>` 를 넣어주고 추가 포함 디렉터리에는 jemalloc의 include 디렉토리를 지정해주고 빌드하면 된다.

이렇게하면 빌드가 성공하고 실행파일에 있는 위치에 jemalloc 빌드 후 생성된 .dll 파일을 같이 넣어주면 된다.

....인줄 알았는데 빌드가 안된다.

코드를 보니 jemalloc.h 파일에서

`#include <string.h>` 부분이 문제가 되었다. 이 문제에 대해 찾아보니...

<https://stackoverflow.com/questions/15512790/error-about-finding-strings-h-in-htmlcxx>

아마 유닉스쪽에서 쓰는 헤더파일일 거라고 한다.

해당 부분을

```cpp
#ifdef _WIN32
#include <string.h>
#else
#include <strings.h>
#endif
```

로 교체하면 잘 작동한다.

![](/assets/images/jemalloc-project.png)

프로젝트의 구성속성을 보면 Debug, Debug-static 식으로 나뉘어 있는데, 전자는 .dll 파일을 쓰는 동적 링크, 후자는 .lib 파일을 쓰는 정적 링크이다. 각자 원하는 것으로 사용.
