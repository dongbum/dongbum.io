---
title: CentOS에서 SVN 서버 설정시 주의할 점
date: 2013-11-28T01:39:26+09:00
categories:
  - Linux
tags:
  - CentOS
  - linux
  - subversion
  - svn
  - 리눅스
  - 서브버전
---
HTTP 프로토콜을 이용해 svn 서버를 운영하고 있었는데 이러다보니 아파치 서버가 없이는 svn을 사용할 수가 없는 상황이 되었다.

향후에는 nginx로 넘어가려고 하는데 아파치가 발목을 잡을 수 있어서 svn:// 프로토콜로 넘어가려고 해서 svnserve를 yum으로 설치했으나 svn에 연결이 되지 않았다. 외부 서버에서 접속이 안되는게 아니라 내부에서도 접속이 안되었기 때문에 뭔가 문제라고 생각했다.

곰곰이 생각해보니 svn 서버를 설정할 때 레포지토리 루트 경로를 설정한 적이 없어서 도대체 어디서 레포지토리 루트 경로를 잡고 있는지 궁금했는데 인터넷 검색을 하다가 찾았다.

<http://blog.naver.com/haengro?Redirect=Log&logNo=40194678804>

본문에 있던대로 `/etc/sysconfig/svnserve` 파일을 생성하고 옵션을 설정하니 정상적으로 접속이 되었다.
