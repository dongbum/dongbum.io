---
title: boost::property_tree의 위험성
date: 2017-11-24T14:27:54+09:00
categories:
  - boost
  - C/C++
tags:
  - boost
  - property_tree
---
.ini 파일을 파싱하기 위해 `boost::property_tree` 클래스를 사용했었다.

서버의 성능을 테스트하던 도중 어디사 CPU를 많이 잡아먹나 계속 테스트했는데 네트워크 전송 부분이 계속 문제였다. 아무리 수정을 해도 고쳐지지 않는 상황. 비주얼스튜디오의 성능프로파일러를 돌려보니 boost::property_tree 가 문제의 원인이었다. 환경설정에서 특정한 값을 가져오기 위해 쓰느 코드가 네트워크 속도를 느리게 만드는데 기여하고 있었다.

![](/assets/images/boost-property-tree-error.png)

이 부분을 수정하고나니 CPU 점유율이 아주 낮아졌다.

boost를 마구 쓰면 안된다는걸 깨닫게되었네.
