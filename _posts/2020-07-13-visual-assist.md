---
title: Visual Assist CPU/스레드 사용량 제어
date: 2020-07-13 11:03:01
categories:
  - C/C++
tags:
  - C++
  - VisualStudio
---

Visual Assist를 사용하다보면 VA Parsing... 이라는 메시지와 함께 많은 파일을 파싱하는 경우를 볼 수 있다.

아마 비주얼어시스트가 참조할 파일을 생성하는 과정이 아닐까 싶은데... 문제는 빌드를 하는 등의 다른 작업을 하고 있을 때 이 작업 때문에 CPU를 몽땅 끌어다 쓴다는 것이다.

이것저것 찾아보다가 WholeTomato 사에서 올린 글을 보았다. 역시나 옵션에 있었다.

난 Visual Studio 2019 를 쓰고 있고 다음과 같은 창을 확인할 수 있었다.

![](/assets/images/visualassist.jpg)

Visual Studio 2019 기준으로 `확장` -> `VAssistX` -> `Visual Assist Options...` -> `Performance` 에 가서 스레드 제한 옵션과 스레드 우선권을 조정하면 된다.


### 참고자료
* <https://support.wholetomato.com/default.asp?W134>
* <https://support.wholetomato.com/default.asp?W700>
