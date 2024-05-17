---
title: 안드로이드 에뮬레이터 HAX 기능 활성화하기
date: 2013-01-20T15:53:26+09:00
categories:
  - Android
tags:
  - Android
  - Atom
  - HAX
  - HAXM
  - Intel
  - 안드로이드
  - 인텔
---

![](/assets/images/intel-logo-icon.jpg)

안드로이드 에뮬레이터를 실행하는데 다음과 같은 메시지가 콘솔창에 뜨는 경우가 있다. 내 상황은 OSX에서 기본적인 이클립스 + ADT이고 안드로이드 에뮬레이터까지만 설치된 상태이다.

```
[2013-01-20 15:21:38 - Emulator] emulator: Failed to open the hax module
[2013-01-20 15:21:38 - Emulator]
[2013-01-20 15:21:38 - Emulator] HAX is not working and emulator runs in emulation mode
```

뭔가 문제가 있다는것 같은데 HAX가 뭔지 모르니 도대체 HAX 모듈이 무엇인가 찾아보았다. 역시나 이미 이 문제에 대해 생각해본 사람들이 있었다. 설명이 잘 나온 블로그글 -> <http://blog.saltfactory.net/187>

결론은 HAX라 함은 Hardware Accelerated Manager 정도인것 같다. CPU의 인텔가상화 기술을 이용하여 에뮬레이터의 작동을 빠르게 해준다.

HAX를 사용하기 위해 <http://software.intel.com/en-us/articles/intel-hardware-accelerated-execution-manager/> 로 들어간다. 프로그램을 다운로드하고 설치한다. 설치용량은 4KB. 설치와 사용에 대해서는 위 블로그 주소에 있다.

난 애초부터 AVD에 Intel Atom x86으로 설정해뒀었고 Use Host GPU를 활성화시켰기 때문에 따로 설정할 것 없이 안드로이드 에뮬레이터를 다시 실행했다. 아마 이 기술이 Intel Atom 프로세서를 설정해줘야만 가상화기술이 적용되어 가속되는 것 같다. 설치가 완료되어서 다시 실행되면 처음 나오던 메시지가 다음과 같이 변경된다.

```
[2013-01-20 15:37:34 - Emulator] HAX is working and emulator runs in fast virt mode
```

이것으로 설치완료. 실행해보니 전보다는 확실히 에뮬레이터의 성능이 아주 좋아졌다!!!! 진즉 한번 찾아볼껄...
