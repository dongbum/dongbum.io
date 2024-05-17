---
title: FLV / F4V 를 위한 MIME 타입 설정
date: 2010-01-30T09:28:16+09:00
categories:
  - Flash/Flex/ActionScript
tags:
  - F4V
  - Flash
  - FLV
  - Mime
  - 플래시
---
일을 하다보면 플래시로 동영상 플레이어를 만들 일이 있는데... 가끔은 동영상 파일을 제대로 올려놓고 액션스크립트가 틀린게 없는데도 재생이 안될때가 있다.

이럴 때 제일 먼저 살펴봐야하는건 웹서버의 MIME 타입이다.

이건 플래시 작업자가 해결 할 수 없는 부분이기 때문에 서버관리자에게 요청해야한다.

플래시 비디오 파일은 FLV와 F4V라는 두가지 포맷이 있다.

웹서버 프로그램(Apache나 IIS 등등)에서 MIME 타입을 지정해줄 때 이렇게 지정해주면 된다.

FLV에 대한 MIME 타입 :: flv-application/octet-stream  
F4V에 대한 MIME 타입 :: video/mp4

물론 설정한 후에는 웹서버 재시작은 필수.
