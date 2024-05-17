---
title: 맥 OSX에서 리눅스의 X-Window에 접속하는 방법
date: 2013-10-12T01:50:24+09:00
categories:
  - Linux
tags:
  - eclipse
  - linux
  - MAC
  - OSX
  - X-Window
  - 리눅스
  - 맥
  - 이클립스
---
리눅스 서버에 이클립스를 설치해서 사용 중이다.

윈도우에서는 MobaXterm을 이용해서 접속하면 X-Window에 대한 처리를 알아서 다 해주기 때문에 신경쓸게 없었다.

맥에서도 리눅스 서버의 이클립스를 사용할 수 없을까 해서 동일하게 X-Window를 이용하려 했으나 역시나 되지 않았다. 여기저기서 찾아본 결과 다음의 과정을 거치면 사용할 수 있었다.

일단 X11 프로그램을 설치해야한다. 파인더를 열고 '응용 프로그램'에 있는 '유틸리티' 폴더에 들어가 'X11'을 실행한다. 실행하면 아마 XQuartz라는 프로그램의 홈페이지인 <http://xquartz.macosforge.org> 로 이동한다. 이 프로그램은 맥에서 X11을 사용할 수 있는 프로그램이다.

이 프로그램을 설치하면 유틸리티 밑에 XQuartz라는 프로그램이 설치된다.

이 프로그램을 설치하고 다시 서버에 접속하여 이클립스를 실행했으나 역시나 실행되지 않았다.

다시 검색해서 다음과 같은 글을 찾았다.

<http://mactips.dwhoard.com/mactips/x11-and-terminal/x11-forwarding>

/etc/sshd_config 파일을 수정해야 한다는 내용이다. 링크의 내용대로 X11Forwarding 항목의 주석을 해제하고 yes로 변경해준다.

마지막으로 재부팅을 한번하고 iTerm2로 서버에 접속하여 이클립스를 실행해보면 내 맥에서 XQuartz가 자동으로 실행되며 X-Window로 접속된 이클립스가 실행된다. 만세!
