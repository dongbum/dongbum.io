---
title: 'tcmalloc 은 왜 VS2015, VS2017에서 안될까'
date: 2018-02-06T16:21:16+09:00
categories:
  - C/C++
tags:
  - C
  - Google
  - tcmalloc
---
tcmalloc 을 사용하기 위해 일주일 이상 삽질하다 알게된 내용.

tcmalloc은 Visual Studio 2015에서 디버그 모드로 사용이 불가능하다. 힙과 관련하여 Assertion이 뜰 때가 있다. 프로그램이 크래쉬나는 것도 아니고 중지되는 것도 아니고 화면이 멈춘듯한 현상이 발생.

물론, Visual Studio 2017에서도 디버그 모드로 사용이 불가능하다.

하지만 릴리즈모드로는 잘 작동한다.

Visual Studio 2017로 간단한 코드를 테스트하여 tcmalloc의 성능을 확인해보았다.

* 테스트코드 : [tcmalloc_test_VS2017](/assets/attach/tcmalloc_test_VS2017.zip)

![](/assets/images/tcmalloc-test.png)

확실히 성능으 더 낫지만 기대했던 것만큼 드라마틱한 성능향상은 아니었다. 아마 멀티스레드라면 더 성능격차가 벌어질듯하다.
