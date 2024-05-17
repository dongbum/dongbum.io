---
title: Disable cockpit HTTPS SSL
date: 2020-06-17 13:50:38
categories:
  - Linux
tags:
  - Linux
  - CentOS
---

CentOS 8 을 설치하고 보니 Cockpit 이라는 웹관리 시스템이 추가가 되었다. 마치 옛날에 쓰던 Webmin 과 비슷한데... 실제로 써보니 Webmin 에는 비교할 수 없을만큼 기능이 부족하다. 관리용도로는 별 의미를 두지 말아야할듯.

어찌됐던 이 Cockpit 은 설치도 쉽고 접속도 편하게 할 수 있는데, 접속하자마자 크롬에서 https 인증서 경고를 띄운다.

`/usr/lib/systemd/system/cockpit.service`

파일을 vim 으로 열고

`ExecStart=/usr/libexec/cockpit-ws ...`

이렇게 ExecStart 부분을 찾아서 `--no-tls` 옵션을 붙여주면 해결 완료.

이제 http 로 접속되기 때문에 인증서 경고가 없어진다.

### 참고자료
* <https://linoxide.com/linux-how-to/install-cockpit-linux-centos-7/>
* <https://www.tecmint.com/install-cockpit-web-console-in-centos-8/>
