---
title: CentOS에서 GIT 설치
date: 2013-09-13T00:39:14+09:00
categories:
  - GIT
  - Linux
  - Programming
tags:
  - CentOS
  - GIT
  - linux
  - 리눅스
---
회사의 형상관리시스템이 Subversion에서 GIT를 쓰기로 결정되어 GIT를 내 서버에 한번 설치해본다.

CentOS 6.4에서 yum으로 기본적인 패키지는 다 있을거란 가정하에 설치시작.

yum으로 설치하려 했으나 역시 공식 레포지토리에는 없다. 검색해보니 비공식 레포지토리를 이용해서 설치가 가능하긴하나 개인적으로 비공식 레포지토리는 EPEL을 제외하고는 좋아하지 않으므로 그냥 소스설치를 해보도록 한다. 페도라 EPEL에는 GIT가 있긴한데 그렇게 떙기진 않는다. ㅎㅎ

<http://www.git-scm.com> 에서 소스코드를 받아 압축을 푼다.

압축을 푼 후

```console
make configure
./configure --prefix=/usr/local/git --with-openssl --with-libpcre --with-curl --with-expat --with-iconv --with-perl --with-python --with-zlib --with-tcltk
make
make install
```

하면 끝.

git라고 입력해보면 이제 git 명령이 잘 실행된다.
