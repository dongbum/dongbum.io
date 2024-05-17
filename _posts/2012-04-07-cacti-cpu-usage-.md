---
title: Cacti에서 CPU Usage 표시하기
date: 2012-04-07T01:01:11+09:00
categories:
  - Linux
tags:
  - cacti
  - linux
  - 리눅스
  - 모니터링
  - 서버
---
모니터링툴인 Cacti를 설치하면 기본으로 있는 그래픽 템플릿으로 여러가지 모니터링 템플릿이 들어있는데 아쉽게도 여기에는 CPU 사용률에 대한 그래픽 템플릿이 없다. 물론 Host MIB - CPU Utilization이나 ucd/net - Load Average 같은 템플릿이 있지만 이것은 내가 원하는 ##%형태의 숫자를 보여주는 것이 아니었다.

그래서 역시 오늘도 구글님에게 물어본 결과 괜찮은 답변을 받았다. 역시나 이미 나와 같은 고민을 했던 외국인들이 있었다.

<http://forums.cacti.net/about15412.html>

글을 읽어보면 2-way system / 4-way system / 8-way system 으로 파일이 나뉘어있다. 서버의 CPU에 맞도록 다운로드 받는다. 리눅스 시스템의 경우에는 /proc/cpuinfo 파일에 프로세서 정보가 나와있으니 여기에 CPU가 몇개나 보이는지 확인한 후 적당한 것을 다운로드 받으면 된다.

내 서버의 경우에는 Intel Xeon E3-1220 프로세서를 사용하고 있고 1개의 CPU에 4개의 코어가 장착되어있으므로 4-way용으로 다운 받았다.

다운로드 받은 후 Cacti의 메뉴 중 import templete에 들어가서 다운로드 받은 xml 파일을 선택하면 cacti에 CPU 사용률을 표시할 수 있는 그래픽 템플릿이 생성된다.
