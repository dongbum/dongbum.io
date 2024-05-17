---
title: Python stdlib source files not found 에러 해결방법
date: 2012-11-27T01:37:20+09:00
categories:
  - Python
tags:
  - eclipse
  - MAC
  - OSX
  - python
  - stdlib
  - 이클립스
  - 파이썬
---
파이썬 개발환경을 만들기 위해 이클립스에 PyDev를 설치하고 환경을 맞추다보니 에러가 생긴다.

문제 화면은 다음과 같다.

![](/assets/images/pydev-error.png)

Python stdlib source files not found. 라는 에러인데... (파이썬도 stdlib 라는 라이브러리가 있나보다. 신기하다.) 맥에는 파이썬이 기본으로 설치되어 있는데 왜 안될까 해서 쉘에서 설치여부를 확인해봐도 여전히 설치되어 있음. 미치고 환장할 일이다.

구글을 한참 뒤지고 stackoverflow로 한참 뒤지다보니 여러가지 해결방법이 보인다. 파이썬 홈페이지 가서 파이썬 패키지를 다시 깔면 된다고 하는데 이렇게 하면 파이썬 버전이 3버전대로 업그레이드 되는게 문제다. 난 지금 있는 맥의 환경이 좋은데다가 앞으로 할 작업도 2.7 이하 버전이 필요하기 때문에 이 방법은 패쓰.

그러다가 한가지 URL을 찾았다.

<http://stackoverflow.com/questions/11702139/pydev-debugger-unable-to-find-real-location-for-python-2-7-after-os-10-8-upgrad>

Mac OSX 10.8로 업그레이드 한 경우에는 Command Line Tools를 설치해야한다는 것. 나도 10.7에서 업그레이드해서 설치한 것이라 이 링크가 도움이 될 것 같았다. 밑에 어떤 사람이 리플로 Xcode가 설치되어 있다면 Preference에 Download에 가면 된다고 해서 Xcode를 켜고 가보니 정말로 Install 버튼이 활성화 되어 있었다. 이것을 설치하고 나니 위 에러가 싹 사라졌다.

만약에 Xcode가 없는 사람이라면 <https://developer.apple.com/downloads/index.action?=command%20line%20tools> 에서 다운로드 받을 수 있다.

다시 이클립스를 켜고 PyDev 프로젝트를 생성해보니 잘 된다! 이로써 파이썬 2.7 설치환경은 완료.
