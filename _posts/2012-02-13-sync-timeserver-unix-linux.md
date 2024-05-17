---
title: 유닉스/리눅스 시스템에서 시간 맞추기
date: 2012-02-13T03:24:11+09:00
categories:
  - Linux
tags:
  - hwclock
  - rdate
  - 리눅스
  - 시간설정
  - 타임서버
---
서버의 시간이 date로 봤을 때와 hwclock 으로 봤을 때가 무려 4분이나 차이가 나서 유닉스/리눅스 시스템에서 시간을 맞추는 방법을 찾아보았다.

가장 간편한 rdate와 hwclock 명령을 이용한다.

타임서버의 시각을 알아보려면

```
rdate -p 타임서버주소
```

를 입력한다. 우리나라에서라면

```
rdate -p time.bora.net
```

을 입력하면 보라넷 타임서버의 시각을 표시해준다.

서버와 시간을 동기화하려면

```
rdate -s time.bora.net
```

을 입력한다. 그리고

```
hwclock -w
```

명령을 실행해서 하드웨어 시각까지 맞춰준다.

사실 한번 해주면 그다음부터는 오차가 거의 나지 않지만 하루에 한번씩 주기적으로 설정해주기 위해서 cron.daily 디렉토리에 쉘스크립트 파일을 작성한다.

```shell
#!/bin/sh
rdate -s time.bora.net
hwclock -w
```

이제 24시간 단위로 타임서버의 값을 받아와서 갱신한다.

주의할 점은, 보통 Windows 계열 운영체제에서 쓰는 타임서버인 time.windows.com 이나 MAC 에서 쓰는 타임서버인 time.asia.apple.com 에는 접속이 안되더라. 모두다 Time out 에러를 보여줬다. 아마 타임서버에서 요청하는 곳의 주소를 판단하거나 하는게 아닌가 싶다.
