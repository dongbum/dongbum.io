---
title: CentOS에서 Jenkins 설치하기
date: 2013-11-17T14:28:30+09:00
categories:
  - C/C++
  - Java/JSP
  - Linux
  - Programming
tags:
  - CentOS
  - CI
  - Jenkins
  - linux
  - Tomcat
  - 리눅스
  - 젠킨스
---
![](/assets/images/jenkins_logo-1.png)

CentOS에서 Jenkins를 설치하기 위한 문서이다.

CentOS 6.4이고 Jenkins는 2013년 11월 17일 버전으로 한다.

젠킨스를 설치할 디렉토리를 설정하고 거기에 젠킨스 홈페이지인 http://jenkins-ci.org 에서 최신버전을 받아 넣는다. 현재 가장 최신 버전은 1.539 버전이다. 다운로드 받을려면

```
wget http://mirrors.jenkins-ci.org/war/latest/jenkins.war
```

명령으로 war 파일을 받을 수 있다. 내 경우에는 /home/jenkins 디렉토리를 만들고 거기에 war 파일을 받아 넣었다.

톰캣의 환경설정을 한다. 내 경우에는 톰캣을 아파치와 연동해서 사용하고 있다.

톰캣의 서버 설정파일인 /etc/tomcat6/server.xml 에 가서 다음의 내용르 추가한다.

```xml
<Host name="jenkins.도메인.com" appBase="/home/jenkins" unpackWARs="true" autoDeploy="true" xmlValidation="false" xmlNamespaceAware="false">
 <Alias>jenkins.도메인.com</Alias>
</Host>
```

난 Alias를 해놨기 때문에 Catalina 디렉토리의 jenkins.도메인.com 디렉토리의 ROOT.xml 파일을 수정해야했다. ROOT.xml 파일에는 다음의 내용을 넣는다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
 <Context path="" docBase="" debug="1">
 </Context>
```

만약 Alias를 쓰지 않는다면 위의 server.xml 설정 파일 중 `<Alias></Alias>` 부분을 위의 내용으로 바꾸어주면 된다.

service tomcat6 restart 명령으로 톰캣을 재시작한다. 재시작하고 웹브라우저를 연다음 아까 설치한 http://jenkins.도메인.com:8080/jenkins 으로 접속해본다.

안된다...

방화벽의 8080 포트를 열어줘야한다. /etc/sysconfig/iptables 에서 8080 포트를 열어주고 service iptables restart 를 입력하여 방화벽을 재시작한다.

다시 http://jenkins.도메인.com:8080/jenkins 으로 접속하면 다음과 같은 화면이 뜬다.

![](/assets/images/jenkins_error.png)

`JENKINS_HOME` 이 설정되어 있지 않아 다른 디렉토리를 설정한다는 내용이다.

war 파일을 넣어놓은 디렉토리에 젠킨스 홈디렉토리롤 쓸 디렉토리를 추가한다. 내 경우에는 /home/jenkins/.jenkins 로 정했다.

그다음 /etc/profile에 다음의 내용을 추가한다.

```
export JENKINS_HOME=/home/jenkins/.jenkins
```

바로 적용을 위해서 `source /etc/profile` 을 입력한다. 내용이 적용되면 톰캣을 재시작한다.

다시 같은 위치로 들어가보면 다음처럼 젠킨스 사용 준비가 완료된다.

![](/assets/images/jenkins_ready.png)

이제 젠킨스를 사용하면 된다.
