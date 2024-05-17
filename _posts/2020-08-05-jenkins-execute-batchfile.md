---
title: 젠킨스에서 배치파일 실행시 주의할 점.
date: 2020-08-05 11:18:01
categories:
  - C/C++
tags:
  - Jenkins
---

젠킨스의 빌드 과정 중 `Execute Windows batch command`로 스크립트를 작성할 때 다른 배치파일을 호출해야할 때가 있다. 이 경우 그냥 `aaa.bat`처럼 배치파일명만 입력하면 아래와 같은 에러메시지를 받게 된다.

```
The system cannot find the path specified.
The system cannot find the path specified.
The system cannot find the path specified.
The system cannot find the path specified.
The system cannot find the path specified.
Build step 'Execute Windows batch command' marked build as failure
Performing Post build task...
Could not match :ERROR  : False
Logical operation result is FALSE
```

이런 경우에는 `call aaa.bat`처럼 앞에 `call` 명령을 붙이면 정상 호출된다.
