---
title: Google Breakpad 설치 (1)
date: 2016-06-29T11:24:48+09:00
categories:
  - C/C++
  - Programming
  - Python
tags:
  - Breakpad
  - C
  - Google
  - python
---
Google Breakpad를 사용하기 위한 방법에 대해 고생했던 것을 정리한다.

윈도우와 리눅스에서 동시에 사용할 수 있는 서버프로그램을 개발하고 있는데 윈도우에서야 미니덤프를 이용하여 덤프를 남기면 되지만 리눅스에서는 coredump라는 생소한 시스템을 이용해야해서 아예 크로스플랫폼 덤프 시스템을 찾다가 구글브레이크패드를 이용해봐야겠다는 생각에 시작했다.

<https://chromium.googlesource.com/breakpad/breakpad> 로 이동한다. 많은 블로그에서 <http://google-breakpad.googlecode.com/svn/trunk> 에서 체크아웃 받으라고 나와있지만 이것은 옛날 정보이다. 현재를 기준으로 사이트는 이동되었으며 SVN이 아니라 GIT을 이용해야 소스를 받을 수 있다.

GIT을 이용하면 되긴하나 귀찮으므로 master 브랜치를 선택하고 tgz로 압축된 파일을 받는다. 7zip을 이용하여 압축파일의 압축을 해제한다. 윈도우에서는 tgz 파일을 풀어서 tar 파일을 만들고 다시 또 한번 아카이빙을 풀어야한다. 물론 7zip 하나로 다 가능하다.

프로그램 설명서를 보기 위해 doc 폴더로 이동을 하면.... 아오... 마크다운 형식의 .md 파일만 잔뜩 들어있다. 브라우저로 볼 수가 없으므로 귀찮아서 그냥 구글 브레이크패드 홈페이지의 도큐먼트를 읽어보면 된다.

....근데 모르겠다.

대충 다른 블로그를 찾아보니 gyp 라는 시스템으로 비주얼스튜디오 솔루션 파일을 생성하면 되고.... gyp는 오픈소스이고... 구글 브레이크패드 소스를 받으면 포함되어 있단다. 근데 내가 보기에는 아무리 찾아도 gyp 라는 시스템이 포함되어 있진 않은 것 같다.

구글에 찾아보니 <https://chromium.googlesource.com/external/gyp> 에 가면 받을 수 있덴다.

가보니 또 git으로 받으라고 한다. 그냥 tgz 파일을 받고 압축을 푼다. 이 프로그램은 홈페이지에 도큐먼트도 없고 도움말도 없다. 뭐 어쩌라는건지...?

한참 애먹은 끝에... 일단 파이선을 설치하고(일단 최신버전인 3.5.2로 설치했다.), cmd 를 관리자권한으로 열고, python setup.py install 을 실행하면 된다는걸 알아냈다. 아오 짜증... 여튼 명령어를 입력하면 뭔가 텍스트가 촤르르륵 올라간다.

자 이제 다시 구글 브레이크패드를 다운 받은 폴더로 이동해서... src/build 에 있는 all.gyp를 실행하기 위해 gyp all.gyp 를 입력한다.

.....는 실패. 문법 오류가 있다고 한다.

그럼 다시 src/client/windows 의 breakpad_client.gyp 를 실행해본다.

...는 실패. 문법 오류가 있다고 한다.

파이선버전 문제인가 싶어서 3.5.2 버전을 다 지우고 2.7.12 버전으로 다시 설치하고 아까 all.gyp 명령을 다시 실행해본다.

안된다. 파이선을 재설치하는 과정에서 gyp가 다 삭제되었다. 다시 gyp를 설치한다.

다시 해봤는데 똑같다... 아오 힘들어.

<http://yardbirds.tistory.com/107> 를 보니

> src\client\windows 폴더에서 ..\..\tools\gyp\gyp.bat breakpad_client.gyp 를 실행하면 솔루션 파일과 프로젝트 파일이 생성되는 것을 볼 수 있다.

라고 한다. 해봤다.

```console
D:\Library\google-breakpad\src\client\windows>d:\Library\gyp-master\gyp.bat breakpad_client.gyp
gyp: Cycles in .gyp file dependency graph detected:
Cycle: breakpad_client.gyp -> sender\crash_report_sender.gyp -> breakpad_client.gyp
Cycle: unittests\client_tests.gyp -> breakpad_client.gyp -> unittests\client_tests.gyp
Cycle: unittests\client_tests.gyp -> crash_generation\crash_generation.gyp -> breakpad_client.gyp -> unittests\client_tests.gyp
Cycle: breakpad_client.gyp -> crash_generation\crash_generation.gyp -> breakpad_client.gyp
Cycle: unittests\client_tests.gyp -> handler\exception_handler.gyp -> crash_generation\crash_generation.gyp -> breakpad_client.gyp -> unittests\client_tests.gyp
Cycle: breakpad_client.gyp -> handler\exception_handler.gyp -> crash_generation\crash_generation.gyp -> breakpad_client.gyp
Cycle: breakpad_client.gyp -> tests\crash_generation_app\crash_generation_app.gyp -> handler\exception_handler.gyp -> crash_generation\crash_generation.gyp -> breakpad_client.gyp
```

갑자기 무슨 사이클을 돈다는 개소리를 하면서 안된다. 구글에서 Cycles in .gyp file dependency graph detected 라는 문장으로다시 검색.

<http://stackoverflow.com/questions/2925094/how-to-build-google-breakpad> 를 보니 `--no-circular-check` 옵션을 붙이면 된단다. 옵션을 붙였더니 그제서야 비주얼스튜디오 솔루션 파일이 생성되었다.

아이고 힘들다...

**2018년 12월 25일 추가**

위 작업에서 파이썬 3.5에서는 확실히 되지 않았다. 파이썬 2.7로 하니 제대로 작동하였다. 반드시 파이썬 2.7로 해볼 것. 파이썬은 도대체 왜 이따위로 만들어져 있는건지 정말 이해가 안간다...
