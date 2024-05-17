---
title: 리눅스에서 스왑 메모리 관리하기
date: 2016-12-10T12:58:45+09:00
categories:
  - Linux
---

가상서버로 이사한 이후 메모리가 너무 적은지 계속 메모리 관련 에러가 난다.

passenger 모듈 설치에도 메모리에러 때문에 잘 안되었었는데 redmine + git을 사용하려고 하니 또 메모리가 너무 적다고 안된다고 한다. 아무래도 스왑메모리를 2기가 정도 지정해놓고 계속 써야할 모양.

`dd if=/dev/zero of=/swap bs=1024 count=2048000`

명령으로 2기가짜리 빈 파일을 만든다.

`mkswap /swap`

명령으로 스왑 생성

`swapon /swap`

명령으로 스왑 활성화

`/etc/fstabs` 에 추가

`/swap swap swap defaults 0 0`
