---
title: 윈도우 서비스 강제종료
date: 2018-09-17T15:25:01+09:00
categories:
  - GameServer
tags:
  - 강제종료
  - 서버
  - 윈도우서비스
---
윈도우 서비스로 작동 중인 서버프로그램이 멈출 때의 해결방법.

* 원본출처 : http://zewtion.tistory.com/294

`tasklist`

명령어로 pid 확인. 마치 리눅스의 ps 명령어의 역할.

`taskkill /f /pid 0000`

확인된 pid를 이용해 서비스를 강제종료시킨다.
