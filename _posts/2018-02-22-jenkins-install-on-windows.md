---
title: Windows에서 Jenkins 설치하기
date: 2018-02-22T16:39:35+09:00
categories:
  - C/C++
  - Java/JSP
tags:
  - Jenkins
  - Tomcat
  - Windows
  - 윈도우
  - 젠킨스
  - 톰캣
---
CentOS에서 Jenkins를 설치하는 글을 썼었는데 이제는 윈도우에서 설치해보기로 했다.

[Jenkins 홈페이지](https://jenkins.io)에 보면 젠킨스를 여러가지 버전으로 제공하고 있다. [각 운영체제에 맞도록 인스톨러를 제공](https://jenkins.io/download/)하고 있다. 톰캣 따로 설치해서 설정하는게 귀찮아서 그냥 Windows Stable Installer 버전으로 다운로드 받고 설치한다.

http://127.0.0.1:8080 에 접속해보면 젠킨스 화면이 나온다.

![](/assets/images/jenkins-install-init-screen.png)

빨강색 글씨에 있는 경로를 찾아사 initialAdminPassword 의 암호를 입력해준다. Setup Wizard가 실행되면 따라서 진행한다. 플러그인을 수동으로 선택하여 설치할지 추천하는 플러그인을 설치할지 나오는데 난 그냥 귀찮아서 제안하는대로 설치했다.

플러그인을 잠시 설치하고 나면 관리자 계정을 생성한다. 관리자 계정까지 생성하고 나면 http://localhost:8080/ 로 들어가면 젠킨스가 실행된다.

### 2019년 3월 6일 추가

윈도우용 톰캣을 설치하고 webapps 디렉토리에 jenkins.war 파일을 넣고 실행하면 위와 같은 화면이 나오지만 initialAdminPassword 파일의 경로가 다르다.

```
C:\WINDOWS\system32\config\systemprofile\.jenkins\secrets\initialAdminPassword
```

이 경로의 파일을 요구하게 된다. 하지만 이 경로에 들어가봐도 이 파일은 존재하지 않는다.

이런 경우에는 로그 파일을 찾아봐야 한다. jenkins 디렉토리에서 logs 디렉토리로 간다음 catalina 로그파일을 열어서 password 로 검색해보면 생성된 암호를 찾을 수 있었다.
