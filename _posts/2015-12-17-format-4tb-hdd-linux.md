---
title: 리눅스에서 4TB 하드디스크 파티션하고 포맷하는 방법
date: 2015-12-17T22:28:22+09:00
categories:
  - Linux
tags:
  - CentOS
  - Fdisk
  - linux
  - Parted
  - 리눅스
  - 파티션
---
CentOS 7을 설치하고 가지고 있는 하드디스크들을 장착하고 파티셔닝을 진행했다.

500GB 하드디스크와 250GB 하드디스크는 fdisk 명령어로 파티션을 나누고 mkfs.xfs 명령어로 포맷하고 마운트하니 제대로 인식되고 사용 가능했다. 그런데 새로 구입한 4TB 하드디스크도 같은 방법으로 파티셔닝하고 파일시스템 생성 후 마운트하니 2TB로 잡혔다.

왜 이런가 찾아보다가 발견.

![](/assets/images/hdd.png)

내용은 이 디스크는 4TB이며 parted를 이용하라는 말이다.

parted 사용법은 다음 링크를 참조했다.

  * <http://freekang.tistory.com/427>
  * <http://blog.naver.com/romanst/220547472292>

parted를 통해서 무사히 파티셔닝을 마쳤다.
