---
title: yum에서 특정패키지 제외하고 업데이트하는 방법
date: 2018-09-18T11:23:32+09:00
categories:
  - Linux
tags:
  - yum
  - yum update
  - 리눅스
  - 업데이트
  - 패키지
---
지금 운영하는 서버에는 데디케이트 서버 라이브러리를 제작해준 회사에서 제공한 패키지가 같이 설치되어 있다.

이 파일이 따로 RPM 파이롤 제공되었기 때문에 yum localinstall 로 설치하게 되었는데, 문제는 yum update 를 하게 되면 이 패키지까지 같이 업데이트를 하려고 시도를 하게 된다.

이 패키지는 강제설치된 것이어서 의존성 문제가 엄청나게 발생하고 결국에는 yum update 는 실패하게 된다.

그래서 매번 yum update 를 할때마다 이 패키지만 제외한 채로 하나씩 업데이트를 해왔는데 아예 이 패키지를 제외하고 업데이트를 간편하게 하는 방법이 있었다.

원본 출처 : https://access.redhat.com/solutions/10185

yum update 시에

```
yum update --exclude=PACKAGENAME
```

을 이용해 특정패키지를 제외하고 나머지 패키지를 전체 업데이트 할 수 있다.

하지만 매번 명령어를 저렇게 입력해야하는지라, 아예 저 명령조차 입력하고 싶지 않다면,

`/etc/yum.conf` 파일에 다음처럼 입력한다.

```
[main]
cachedir=/var/cache/yum/$basearch/$releasever
keepcache=0
debuglevel=2
logfile=/var/log/yum.log
exclude=kernel* redhat-release*
```

이렇게 추가하고 나면 이후에는 아예 yum update 시에 이 패키지를 제외하게 된다.
