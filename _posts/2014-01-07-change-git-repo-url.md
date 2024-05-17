---
title: git 저장소 주소 변경하기
date: 2014-01-07T17:03:59+09:00
categories:
  - GIT
tags:
  - GIT
  - origin
---
사내시스템의 아이피 변경으로 소스코드 관리 서버 URL이 변경되었다.

작업 디렉토리에 연결된 저장소 URL을 변경해야해서 찾아보니 [다음](http://www.bemga.com/07-04-2012/%5BGit%5D_origin_%E1%84%8C%E1%85%AE%E1%84%89%E1%85%A9_%E1%84%87%E1%85%A7%E1%86%AB%E1%84%80%E1%85%A7%E1%86%BC%E1%84%92%E1%85%A1%E1%84%80%E1%85%B5.html)과 같은 링크를 발견했다.

현재 작업 디렉토리와 연결된 저장소 URL을 알아내려면

```console
git remote show origin
```

현재 작업 디렉토리와 연결된 저장소 URL을 변경하려면

```console
git remote set-url origin ssh://git서버주소
```

를 입력하면 된다.
