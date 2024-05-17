---
title: 시스템 코드페이지 변경
date: 2013-02-01T23:45:46+09:00
categories:
  - Linux
tags:
  - CentOS
  - Code Page
  - UTF-8
  - 리눅스
  - 코드페이지
---

phpsysinfo를 업그레이드하고 나서 보니 Code Pages 부분 즉, 시스템코드페이지가 euc-kr로 나왔다.

왠만하면 서버의 모든 환경을 UTF-8로 맞추고 있는 실정이라 시스템코드페이지를 UTF-8로 변경하기로 결정.

/etc/sysconfig/i18n 파일을 열어서

```
LANG="ko_KR.UTF-8"
SUPPORTED="ko\_KR.eucKR:ko\_KR:ko"
```

이렇게 변경하고 저장한다.

완벽한 적용을 위해서 서버를 재부팅하면 완료.
