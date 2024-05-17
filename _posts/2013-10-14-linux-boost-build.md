---
title: 리눅스에서 boost 라이브러리 빌드하기
date: 2013-10-14T23:02:36+09:00
categories:
  - C/C++
tags:
  - boost
  - C
  - 라이브러리
  - 부스트
  - 빌드
---
부스트 라이브러리를 리눅스에서 사용하기 위한 절차를 기록해본다.

내 서버의 경우는 CentOS 6.4 버전이며 yum을 이용해 최신의 업데이트가 모두 다 되어있는 상태다.

일단 boost.org에서 최신 소스를 받는다. 이 글을 쓰고 있는 2013년 10월 14일 기준으로 1.54 버전이 가장 최신이다. unix용으로는  tar.gz과 tar.bz2 형식으로 제공되고 있는데 아무래도 압축효율이 높은 bz2 파일을 다운 받았다.

적당한 디렉토리에 받은 파일을 놓고 bunzip2 명령을 이용해 압축을 해제하면 tar 파일이 나온다. tar xvf boost\_1\_54_0.tar 명령을 이용해서 압축을 해제한다.

압축을 해제한 디렉토리로 들어가서 `./bootstrap.sh` 파일을 실행한다.

```console
[root@83rpm boost_1_54_0]# ./bootstrap.sh
Building Boost.Build engine with toolset gcc... tools/build/v2/engine/bin.linuxx86_64/b2
Detecting Python version... 2.6
Detecting Python root... /usr
Unicode/ICU support for Boost.Regex?... not found.
Backing up existing Boost.Build configuration in project-config.jam.1
Generating Boost.Build configuration in project-config.jam...
Bootstrapping is done. To build, run:
./b2
To adjust configuration, edit 'project-config.jam'.
Further information:
- Command line help:
./b2 --help
- Getting started guide:
http://www.boost.org/more/getting_started/unix-variants.html
- Boost.Build documentation:
http://www.boost.org/boost-build2/doc/html/index.html
[root@83rpm boost_1_54_0]#
```

위와 같은 메시지가 나타난다.

메시지에 나타난대로 ./b2 명령어를 입력하면 빌드가 시작된다. 내 서버의 CPU는 Xeon E3 1220 인데도... 꽤나 오래걸린다. 빌드하는 동안 CPU 사용률이 30~50% 정도를 왔다갔다한다. 대략 10~15분 정도 걸리는 것 같다.

```console
...(생략)...
cc.compile.c++ bin.v2/libs/wave/build/gcc-4.4.7/release/link-static/threading-multi/token_ids.o
gcc.compile.c++ bin.v2/libs/wave/build/gcc-4.4.7/release/link-static/threading-multi/wave_config_constant.o
common.mkdir bin.v2/libs/wave/build/gcc-4.4.7/release/link-static/threading-multi/cpplexer
common.mkdir bin.v2/libs/wave/build/gcc-4.4.7/release/link-static/threading-multi/cpplexer/re2clex
gcc.compile.c++ bin.v2/libs/wave/build/gcc-4.4.7/release/link-static/threading-multi/cpplexer/re2clex/aq.o
gcc.compile.c++ bin.v2/libs/wave/build/gcc-4.4.7/release/link-static/threading-multi/cpplexer/re2clex/cpp_re.o
gcc.archive bin.v2/libs/wave/build/gcc-4.4.7/release/link-static/threading-multi/libboost_wave.a
common.copy stage/lib/libboost_wave.a
...updated 1084 targets...
The Boost C++ Libraries were successfully built!
The following directory should be added to compiler include paths:
/backup/program/boost_1_54_0
The following directory should be added to linker library paths:
/backup/program/boost_1_54_0/stage/lib
[root@83rpm boost_1_54_0]#
```

한참 기다리면 위와 같은 메시지가 나온다. 부스트 라이브러리 빌드에 성공했다는 메시지와 컴파일러에서 인클루드할 경로, 라이브러리 경로를 알려주는 메시지이다.

이것으로 끝.

윈도우에서 빌드할 때는 여러가지 옵션을 넣고 한 것 같은데 리눅스에서는 너무 쉽게 끝나서 뭔가 이상하다. 혹시나 이 빌드 방법이 잘못되었다면 다음번에 수정해 나가도록 한다.
