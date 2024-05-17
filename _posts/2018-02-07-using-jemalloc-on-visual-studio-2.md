---
title: Visual Studio에서 jemalloc 설치하고 사용법 (2)
date: 2018-02-07T14:08:56+09:00
categories:
  - C/C++
tags:
  - C
  - cygwin
  - cygwin64
  - jemalloc
  - tcmalloc
  - Visual Studio
  - 메모리
---
> 이 글은 jemalloc 을 설치하다가 애먹은 경험으로 쓰는 것. 혹시 나처럼 Visual Studio에서 jemalloc을 쓰려고 고생하는 사람들에게 도움이 되길 바란다. 그리고 퍼갈 때에는 출처도 밝혀주시기를...

일단 <https://github.com/jemalloc/jemalloc> 에서 소스코드를 Clone 한다. (내 경우에는 `D:\Library\jemalloc` 으로 다운로드했다.)

master 브랜치를 다운로드하고 안에 들어가서 살펴보면 msvc 라는 디렉토리가 있고 그 안에 jemalloc_vc2015.sln 파일이 있다. Visual Studio 2015용 솔루션이 있으니 얼마나 감사한가.

솔루션을 열어보면 이미 jemalloc 프로젝트와 test_threads 프로젝트가 추가되어있다.

![](/assets/images/jemalloc-rebuild.png)

jemalloc 프로젝트를 빌드해보면...

![](/assets/images/jemalloc-build-error-c1083.png)

```
C1083 포함 파일을 열 수 없습니다. 'jemalloc/internal/jemalloc_preamble.h': No such file or directory jemalloc d:\library\jemalloc\src\witness.c 2
```

오류와 함께 빌드가 되지 않는다. 실제로 저 경로에 가서 살펴보면 jemalloc_preamble.h 파일이 없다.

솔루션 파일에 추가되어 있는 ReadMe.txt 파일을 열어보면 다음과 같은 내용이 있다.
```
How to build jemalloc for Windows
=================================

1. Install Cygwin with at least the following packages:
* autoconf
* autogen
* gawk
* grep
* sed

2. Install Visual Studio 2015 with Visual C++

3. Add Cygwin\bin to the PATH environment variable

4. Open "VS2015 x86 Native Tools Command Prompt"
(note: x86/x64 doesn't matter at this point)

5. Generate header files:
sh -c "CC=cl ./autogen.sh"

6. Now the project can be opened and built in Visual Studio:
msvc\jemalloc_vc2015.sln
```

메뉴얼이 있으니 따라가야지.

Cygwin을 구글에서 검색해서 설치한다. Cygwin 설치법은 검색해서 찾자.

하나 참고해둘 것은 Cygwin은 설치프로그램을 실행해서 구성파일을 인터넷에서 다운로드하며 설치한다. 근데 **이 과정이 무지막지하게 느리다.** 기가인터넷이고 지랄이고 이런거 소용 없더라. 미러를 선택할 때 ftp.kaist.ac.kr을 선택했지만 그래도 느리다. 한참 설치하더니 몇가지 패키지가 설치 안되었다고 나온다. 설치프로그램을 다시 실행시켜서 미러를 ftp.jaist.ac.jp 로 바꾸고 다시 설치. 이 과정을 몇번 반복해서야 겨우 설치 완료했다.

Cygwin을 디폴트로 설치하고나서 설치프로그램의 검색창을 이용해 위 메뉴얼에 있는 autoconf, autogen, gawk, grep, sed를 설치해줘야 한다. 잊지말것.

아마 Cygwin을 처음 보거나 잘 쓰지 않는 사람이라면 이 과정에서 굉장히 스트레스 받을 것이다. 나도 Cygwin 설치에만 하루 넘게 걸렸다. 젠장...

이걸 설치하고 나서는 메뉴얼대로 제어판을 열고 '시스템'의 '고급 시스템 설정'의 '환경 변수'에 가서 '변수' 중 Path 항목의 값에 Cygwin의 bin 경로를 추가해준다. 내 경우에는 `C:\cygwin64\bin` 을 추가해줬다.

이제 시작메뉴에서 'VS2015 x64 네이티브 도구 명령 프롬프트'를 실행한다.

경로를 jemalloc이 설치된 폴더로 이동한다. 내 경우에는 `D:\Library\jemalloc` 으로.

메뉴얼에 있는대로 `sh -c "CC=cl ./autogen.sh"` 를 입력한다.

![](/assets/images/jemalloc-compile.png)

각종 환경설정 사항을 체크하고 컴파일 과정이 지나간다. (다중프로세서 컴파일은 하지 않는듯하다. 적당히 웹서핑하며 몇분 기다리면 된다.)

![](/assets/images/jemalloc-compile-complete.png)

이 화면이 나오면 컴파일이 끝난 것이다.

아까 jemalloc_preamble.h 파일이 없던 경로에 가보면 jemalloc_preamble.h 파일이 생성되어 있다.

이제 다시 Visual Studio 2015를 켜서 솔루션을 열고 jemalloc 프로젝트를 빌드해본다.

![](/assets/images/02/jemalloc-apply.png)

당연히 잘된다. ㅎㅎㅎ

결론 : Cygwin을 설치하는게 제일 애먹었다. 이것만 설치하고 나머지는 메뉴얼대로 설정하면 된다.
