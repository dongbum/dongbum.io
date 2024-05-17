---
title: PCH 컴파일러 에러
date: 2020-06-05 14:15:38
categories:
  - C/C++
tags:
  - C++
---

Visual Studio 2017 사용 중 가끔 가다가 아래와 같은 에러가 발생한다.

```
9>이 솔루션 구성에 대해 빌드하도록 선택된 프로젝트가 없습니다.
10>------ 빌드 시작: 프로젝트: GameServer (6. Server\GameServer), 구성: Release x64 ------
10>RzAlchemyManager.cpp
10>c1xx : error C3859: PCH에 대한 가상 메모리를 만들지 못했습니다.
10>c1xx: note: PCH: 요청된 메모리 블록을 가져올 수 없습니다.
10>c1xx: note: 자세한 내용은 https://aka.ms/pch-help를 참조하세요.
10>c1xx : fatal error C1076: 컴파일러 한계: 내부 힙 한계에 도달했습니다.
10>"GameServer.vcxproj" 프로젝트를 빌드했습니다. - 실패
========== 빌드: 성공 0, 실패 1, 최신 13, 생략 9 ==========
```

해결 방법은 *프로젝트 설정 -> 구성 속성 -> C/C++ -> 명령줄에서 -Zm200을 추가* 해주면 해결된다. 해결되지 않는다면 수치를 더 증가시켜보자.
