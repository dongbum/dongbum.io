---
title: Zookeeper 설치
date: 2013-09-09T22:49:37+09:00
categories:
  - C/C++
  - Linux
  - Programming
tags:
  - linux
  - Zookeeper
  - 리눅스
  - 주키퍼
---
![](/assets/images/zookeeper.jpg)

회사 서버시스템에 적용 할 수 있나 알아보기 위해 Zookeeper를 테스트하기로 한다.

일단 아파치-주키퍼 사이트(<http://zookeeper.apache.org/>)에 들어가 다운로드 링크를 찾아 wget 으로 다운로드한다. 지금 이 포스팅을 하는 시점에서 가장 최신 버전은 3.4.5 버전이다.

압축을 풀면 나오는 디렉토리가 주키퍼의 프로그램 디렉토리이다.

일단 conf 디렉토리로 가서 zoo_sample.cfg 파일을 zoo.cfg 파일로 복사한다.

그리고 bin 디렉토리로 가서 ./zkServer.sh 명령으로 주키퍼를 실행한다.

실행하면 다음과 같은 화면이 출력된다.

```console
JMX enabled by default
Using config: /backup/program/zookeeper-3.4.5/bin/../conf/zoo.cfg
Usage: ./zkServer.sh {start|start-foreground|stop|restart|status|upgrade|print-cmd}
```

CentOS의 service 와 마찬가지로 실행옵션을 주면서 실행할 수 있다.

./zkServer.sh start 를 입력했다.

```console
JMX enabled by default
Using config: /backup/program/zookeeper-3.4.5/bin/../conf/zoo.cfg
Starting zookeeper ... STARTED
```

시작되었다고 나왔지만 실제로 프로세스는 떠있지 않았다.

그래서 뭘까 생각해보다가 bin 디렉토리를 보았더니 zookeeper.out 파일이 생성되어 있었다. 이 파일의 내용에는,

```
nohup: failed to run command '/usr/share/java/bin/java': 그런 파일이나 디렉터리가 없습니다
```

라고 되어있다.

아마 환경설정은 zkServer.sh 파일 안에 있을 것이므로 이 파일을 편집에서 java 라는 단어로 검색했으나 문제가 될만한 부분이 없었다. 하지만 보다보니 zkEnv.sh 를 실행하는 부분이 보인다. 파일명으로 보아 환경설정을 파는 파일이 이 파일 같으므로 다시 이 파일을 vi로 열어본다.

중간쯤 가다보면

```shell
if [ "$JAVA_HOME" != "" ]; then
 JAVA="$JAVA_HOME/bin/java"
 else
 JAVA=java
 fi
```

이런 내용이 있다.

자바를 실행할 홈디렉토리를 설정하는 것 같다.

쉘로 나와서 echo $JAVA_HOME 을 입력해보니 /usr/share/java 이라고 뜬다. 그래서 결국은 zookeeper.out 파일에 /usr/share/java/bin/java 이런 경로가 없다고 나온 것이었다.

CentOS 6.4 에서는 java 명령은 사실 경로 설정이 없어도 상관이 없으며 굳이 경로를 넣고 싶다면 /usr/bin/java 로 실행하면 된다. (이 파일은 /etc/alternatives/java 의 심볼링링크이다.)

```shell
if [ "$JAVA_HOME" != "" ]; then
 JAVA="$JAVA_HOME/bin/java
 else
 JAVA=java
 fi
```

이 부분을

```shell
if [ "$JAVA_HOME" != "" ]; then
 # JAVA="$JAVA_HOME/bin/java"
 JAVA=/usr/bin/java
 else
 JAVA=java
 fi
```

처럼 바꾸고 다시 ./zkServer.sh start 를 실행한다. 아무 문제 없이 STARTED 되었다고 나오고, 다시 ./zkServer.sh status 를 입력해보면

```console
JMX enabled by default
 Using config: /backup/program/zookeeper-3.4.5/bin/../conf/zoo.cfg
 Mode: standalone
```

라고 나오면 실행 성공.

이제부터는 주키퍼를 실제로 사용해보도록 한다.
