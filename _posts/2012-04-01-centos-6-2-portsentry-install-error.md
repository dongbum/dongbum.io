---
title: CentOs 6.2에서 PortSentry 설치 시 오류 해결방법
date: 2012-04-01T04:01:45+09:00
categories:
  - Linux
tags:
  - portsentry
  - 포트스캔
---
최근에 중국쪽에서 해킹시도가 감지되어 portsentry를 설치하기로 했다.

portsentry는 서버에 포트스캐닝이 들어오면 해당 아이피를 미리 지정된 정책에 따라 host.deny 파일에 추가하여 tcpwrapper로 차단하거나 iptables 같은 방화벽과 연동하여 해당 아이피를 차단하게 된다. 해킹 시도시 포트스캐닝을 해보고 열린 포트(특히 SSH)를 공격한다는 것을 생각할 때 포트스캐닝 시도시 바로 막아버리는 것은 상당히 좋은 정책인 것 같다.

여하튼 그런 이유로 내 서버에 portsentry를 설치해보았다.

<http://www.psionic.com/products/portsentry.html> 에서 portsentry 프로그램을 일단 다운로드 받는다.

다운로드 받은 파일을 압축을 풀고 make 명령을 실행하래서 해봤더니 사용법은 make '시스템타입'을 입력하면 된다고 한다. 내 경우에는 리눅스이므로 make linux를 입력.

그런데 다음과 같은 오류를 내고 더이상 진행이 되지 않았다.

```
SYSTYPE=linux  
Making  
cc -O -Wall -DLINUX -DSUPPORT_STEALTH -o ./portsentry ./portsentry.c  
./portsentry\_io.c ./portsentry\_util.c  
./portsentry.c: In function ‘PortSentryModeTCP’:  
./portsentry.c:1187: warning: pointer targets in passing argument 3 of ‘accept’ differ in signedness  
/usr/include/sys/socket.h:214: note: expected ‘socklen\_t \* \\_\_restrict\_\_’ but argument is of type ‘int \*’  
./portsentry.c: In function ‘PortSentryModeUDP’:  
./portsentry.c:1384: warning: pointer targets in passing argument 6 of ‘recvfrom’ differ in signedness  
/usr/include/sys/socket.h:166: note: expected ‘socklen\_t \* \\_\_restrict\_\_’ but argument is of type ‘int \*’  
./portsentry.c:1584:11: warning: missing terminating " character  
./portsentry.c: In function ‘Usage’:  
./portsentry.c:1584: error: missing terminating " character  
./portsentry.c:1585: error: ‘sourceforget’ undeclared (first use in this function)  
./portsentry.c:1585: error: (Each undeclared identifier is reported only once  
./portsentry.c:1585: error: for each function it appears in.)  
./portsentry.c:1585: error: expected ‘)’ before ‘dot’  
./portsentry.c:1585: error: stray ‘’ in program  
./portsentry.c:1585:24: warning: missing terminating " character  
./portsentry.c:1585: error: missing terminating " character  
./portsentry.c:1595: error: expected ‘;’ before ‘}’ token  
./portsentry_io.c: In function ‘ConfigTokenRetrieve’:  
./portsentry_io.c:321: warning: cast from pointer to integer of different size  
./portsentry_io.c:324: warning: cast from pointer to integer of different size  
./portsentry_io.c: In function ‘IsBlocked’:  
./portsentry_io.c:670: warning: cast from pointer to integer of different size  
./portsentry_io.c: In function ‘SubstString’:  
./portsentry_io.c:727: warning: cast from pointer to integer of different size  
make: \*** [linux] 오류 1
```

제대로 입력했건만 무엇이 잘못되었을까 한참을 고민하다가 구글에 검색을 하다가 겨우겨우 해결책을 찾았다.

<http://www.howtoforge.com/forums/showthread.php?t=25114>

이 문서에서 나와 동일한 오류를 겪은 사람이 있고 해결책까지 제시되어있었다.

`portsentry.c` 파일의 1584, 1585 행을 보면,

```cpp
printf ("Copyright 1997-2003 Craig H. Rowland <craigrowland at users dot
sourceforget dot net>n");
```

이렇게 두줄로 되어있는데 이것을

```cpp
printf ("Copyright 1997-2003 Craig H. Rowland <craigrowland at users dot sourceforget dot net>n");
```

이렇게 한줄로만 변경해주면 된다.

변경한 후 다시 `make linux` 명령을 내리면 컴파일이 잘 진행된다. 물론 포인터 사이즈에 대한 경고는 여전히 나온다.
