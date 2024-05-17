---
title: 윈도우에서 C++ Boost 사용하기
date: 2014-05-15T00:49:25+09:00
categories:
  - boost
  - C/C++
tags:
  - boost
  - C
  - 부스트
---
윈도우에서 C++ 작업시 유용한 Boost 라이브러리를 사용하기 위해서는 빌드를 해야한다. 파일시스템 등을 다루는 등의 운영체제에 의존하는 기능들을 사용하기 위해 빌드해야한다고 한다.

  1. <http://www.boost.org>에 들어가서 Boost를 다운 받는다.
  2. 압축을 풀고 디렉토리에 들어가 bootstrap.bat 를 실행한다.
     * boost 1.70 이 되면서 **bootstrap.bat msvc** 등처럼 컴파일러를 명시해서 입력해야 한다.
     * 지원되는 컴파일 옵션은 다음과 같다.
    > Toolsets supported by this script are: borland, como, gcc, gcc-nocygwin, intel-win32, metrowerks, mingw, msvc, vc7, vc8, vc9, vc10, vc11, vc12, vc14, vc141

  3. `b2 --help` 를 실행해서 한번 도움말을 쭉 읽어본다.
  4. 윈도우 커맨드창을 열고 해당 다음의 명령을 입력하여 빌드를 시작한다. 64비트, 디버그/릴리즈 모두, 멀티쓰레드, MPI 사용 안함으로 빌드한다.
  5. `b2 variant=debug,release link=static,shared threading=multi address-model=64 --without-mpi -j8 stage`
  6. 빌드 옵션은 여러가지가 더 있다. 찾아봐서 자기가 필요한대로 설정한다.
  7. `-j8`은 몇개의 작업을 동시에 하는지를 결정한다. 4코어인 경우에는 `-j4`를 해주면 모든 코어를 다 사용하게 된다. 적당하게 설정하는 것이 좋다.

빌드에는 시간이 꽤 걸린다.

다음의 링크를 참조했다.

  * <http://anster.egloos.com/viewer/2157882>
  * <http://jacking.tistory.com/1068>
  * <http://blog.naver.com/lunu/100160768950>
