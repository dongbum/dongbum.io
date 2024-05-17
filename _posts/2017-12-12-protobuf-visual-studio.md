---
title: Visual Studio 에서 C++ 로 사용하기 위한 protobuf 설치
date: 2017-12-12T18:52:32+09:00
categories:
  - C/C++
tags:
  - C
  - Google
  - protobuf
  - Visual Studio
---

![](/assets/images/protobuf.png)

구글 protobuf 를 새로 개발하고 있는 서버에 적용해보려고 찾아보았다. 설치에 대한 링크는 [구글 Protocol Buffer VS 설치](http://blog.naver.com/aigis21/220413192943)라는 글이 보였는데 2015년에 작성된 글이라 그런지 지금과 맞지 않았다. vsproject라는 폴더가 있다고 하는데 이런 폴더 자체가 없었으니.

그래서 새로 빌드해보기로 하고 그 과정을 정리해본다.

구글 protobuf 홈페이지로 이동한다.

<https://github.com/google/protobuf/>

C++에서 사용하기 위해 Protobuf Runtime Installation 항목으로 이동했더니 src 폴더로 가보라고 되어있다.

<https://github.com/google/protobuf/tree/master/src> 에 가서 밑의 README.md 파일 내용을 읽어보니 C++ Installation - Windows 항목이 있어 이 내용을 읽어본다.

> If you only need the protoc binary, you can download it from the release page:
>
> <https://github.com/google/protobuf/releases>
>
> In the downloads section, download the zip file protoc-$VERSION-win32.zip. It contains the protoc binary as well as public proto files of protobuf library.
>
> To build from source using Microsoft Visual C++, see [cmake/README.md](https://github.com/google/protobuf/blob/master/cmake/README.md).
>
> To build from source using Cygwin or MinGW, follow the Unix installation instructions, above.

안내문에서 지시하는대로 cmake/README.md 파일을 열어보았다.

<https://github.com/google/protobuf/blob/master/cmake/README.md>

환경설정을 진행하고 cmake를 다운로드한다. 구글에서 cmake 를 검색하여 Win64용의 cmake 프로그램을 설치.

이미 윈도우용 git이 설치되어 있으므로 **VS2015용 개발자 명령 프롬프트**창을 열고 라이브러리를 모아놓은 폴더에 가서 소스를 다운 받는다. 릴리즈 버전은 지금 최신버전인 3.5.0 으로 받았다. 개발자 명령 프롬프트가 아니라 cmd 창으로 열면 컴파일러인 cl.exe가 인식이 안되기 때문에 오류가 난다.

```
git clone -b v3.5.0 https://github.com/google/protobuf.git
```

폴더가 만들어지면 그 안으로 들어가서 gmock 도 받는다.

```
git clone -b release-1.7.0 https://github.com/google/googlemock.git gmock
```

그 안으로 들어가 googletest 코드도 받는다.

```
git clone -b release-1.7.0 https://github.com/google/googletest.git gtest
```

protobuf를 받은 위치에서 cmake 라는 폴더를 들어간다. 이 밑에 build 라는 디렉토리를 만들고 그 밑에 debug, release, solution 이라는 3개의 디렉토리를 만든다.

debug 폴더로 이동하여 다음 명령 실행

```
cmake -G "NMake Makefiles" -DCMAKE_BUILD_TYPE=Debug -DCMAKE_INSTALL_PREFIX=../../../../install ../..
```

release 폴더로 이동하여 다음 명령 실행

```
cmake -G "NMake Makefiles" -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=../../../../install ../..
```

solution 폴더로 이동하여 다음 명령 실행

```
cmake -G "Visual Studio 14 2015 Win64" -DCMAKE_INSTALL_PREFIX=../../../../install ../..
```

> 위 명령을 실행할 때 참고할 점.
>
> 입력 옵션 중 -DCMAKE\_INSTALL\_PREFIX 는 빌드된 파일들이 출력되게 될 경로이다. 이 경로는 상대 경로로 지정할 수 있으므로 자기가 필요한 경로에 맞도록 변경하면 된다. 내 경우에는 현재 폴더의 상위의, 상위의, 상위의, 상위에 있는 install 폴더로 지정할 것이기 때문에 저렇게 입력했다.

설명서에는 Visual Studio 12 2013 으로 설명되어 있지만 난 VS2015를 사용하므로 그에 맞도록 수정했다.

빌드하기 위해 release 폴더혹은 debug 폴더에서 nmake 명령과 nmake check 명령을 차례대로 실행한다.

nmake check 이후 아무 문제 없다면 nmake install 을 진행하면 되나 나같은 경우에는 아래와 같은 에러메시지가 떴다.

```
[----------] Global test environment tear-down
[==========] 2009 tests from 194 test cases ran. (54957 ms total)
[  PASSED  ] 2001 tests.
[  FAILED  ] 8 tests, listed below:
[  FAILED  ] CommandLineInterfaceTest.Win32ErrorMessage
[  FAILED  ] BootstrapTest.GeneratedDescriptorMatches
[  FAILED  ] CsharpBootstrapTest.GeneratedCsharpDescriptorMatches
[  FAILED  ] RubyGeneratorTest.GeneratorTest
[  FAILED  ] TokenizerTest.ParseString
[  FAILED  ] TextFormatMapTest.Sorted
[  FAILED  ] TextFormatTest.Basic
[  FAILED  ] TextFormatExtensionsTest.Extensions
```

이 에러메시지를 해결하기 위해 구글에 검색해봤지만 딱히 좋은 대안은 찾지 못했다. 자연스러운 에러메시지(?)라는 글도 있긴했다.

빌드가 완료되면 lib 파일들과 헤더파일들이 생성되는데 이 생성되는 위치가 protobuf 폴더 바로 위의 install 이라는 폴더에 생성된다.

lib 파일을 비주얼스튜디오에 연결하고 헤더파일을 가져다 사용하면 된다. 이에 대한 내용은 [Protobuf 실제 사용](http://blog.naver.com/aigis21/220413615488) 글을 참조하면 된다. 아니면 검색엔진에 검색해봐도 많이 나온다.

자세한 사용방법은 protobuf 홈페이지에서. <https://developers.google.com/protocol-buffers/docs/cpptutorial>
