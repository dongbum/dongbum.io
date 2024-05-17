---
title: 'boost에서 OpenSSL 사용하기 (1) : 윈도우에서 OpenSSL 빌드'
date: 2019-05-09T14:36:26+09:00
categories:
  - boost
  - C/C++
  - GameServer
tags:
  - boost
  - C
  - OpenSSL
  - SSL
---
boost에서 OpenSSL을 사용하기 위해서는 먼저 OpenSSL 설치가 필요하다. boost의 ssl 관련 파일들에서 openssl 관련 파일들을 인클루드해오기 때문이다.

검색엔진을 찾아보다가, [https://jethro.tistory.com/entry/CC-Windows에서-OpenSSL-써먹기](https://jethro.tistory.com/entry/CC-Windows%EC%97%90%EC%84%9C-OpenSSL-%EC%8D%A8%EB%A8%B9%EA%B8%B0) 를 참고했다.

#### Step 1. ActivePerl 설치

<https://www.activestate.com/products/activeperl/> 로 이동해서 ActivePerl 을 설치한다.

#### Step 2. OpenSSL 소스 다운로드

<https://www.openssl.org/source/> 에서 소스코드를 다운로드한다. 난 1.1.1j 버전을 다운로드했다.

#### Step 3. Configure 설정

```
perl Configure VC-WIN32 --openssldir=C:\OpenSSL-x86-debug no-shared no-asm threads
```

를 입력하라고 하는데 64비트 윈도우이니 VC-WIN64A 로 입력. (VC-WIN64I 도 있긴한데 아마 Itanium 아키텍쳐일 때 쓰라는 것일 것 같다.)

다음과 같은 오류가 발생.

![](/assets/images/openssl-install.png)

dmake나 nmake가 필요한 것 같다.

안내 받은대로 **ppm install dmake** 를 입력하면, 이런저런 패키지들을 설치한다.

다시 configure 명령어 입력.

**perl Configure VC-WIN64A --openssldir=D:\Library\OpenSSL no-shared no-asm threads**

별 문제 없이 끝났다.

#### Step 4. make

그런데 참고하고 있는 블로그 글에는 ms\do_ms.bat 파일이 있다고 했는데 그 위치에 해당 파일이 없다.

구글에 검색해보니...

<https://stackoverflow.com/questions/39076244/why-there-is-no-ms-do-ms-bat-after-perl-configure-vc-win64a>

1.1.0 버전부터는 그 명령어가 없어졌다고 한다. 그냥 nmake 를 입력하라고 되어있다.

nmake를 해보니 그런 명령어가 없다고 한다. 일반 cmd가 아니라 VS2015 x64 네이티브 도구 명령 프롬프트를 다시 실행해야한다.

다시 **nmake** 를 입력하면 빌드가 시작된다.

다 완료된 후 nmake test 를 입력하여 테스트. 테스트도 잘 완료되었다.

nmake install 을 입력했더니...

```console
*** Installing runtime librariesCannot create directory C:/Program Files/OpenSSL: No such file or directoryNMAKE : fatal error U1077: 'C:\Perl64\bin\perl.exe' : '0x2' 반환 코드입니다.Stop.
```

에러로 중지된다. 으어...

구글에 검색해보니 나만 겪는 문제는 아닌듯하다. github에 이슈로 올라와있다.

<https://github.com/openssl/openssl/issues/2034>

댓글을 보고 configure 명령어에 prefix를 추가한다.

**perl Configure VC-WIN64A --prefix=D:\Library\OpenSSL --openssldir=D:\Library\OpenSSL no-shared no-asm threads**

다시 nmake clean, nmake, nmake test 실행.

다시 make install 을 실행하니 제대로 진행되었다.

빌드가 성공되면 prefix로 지정된 위치에 파일들이 생성된다.

OpenSSL 빌드는 끝.
