---
title: PF_INET과 AF_INET의 차이
date: 2012-10-22T16:42:03+09:00
categories:
  - C/C++
  - Note of the wrong answers
  - Programming
tags:
  - AF_INET
  - PF_INET
  - socket
  - 소켓
---
네트워크 프로그래밍 책을 둘러보던 중 소켓 생성할 때 PF_INET으로 생성하는 것을 보고 이게 뭔가 싶었다. 내가 배울 때는 항상 AF_INET으로 배웠기 때문. 책에도 이에 대한 내용이 부족해서 검색해보니 여기에 대한 정보가 많았다.

굳이 정리하자면 PF_INET과 AF_INET은 둘다 상수값 2로 같은 값을 나타내지만 PF\_INET은 프로토콜 설계에, AF_INET은 주소체계에 쓰이는 것이 바람직하다고 한다. 그래서 socket() 함수에서는 PF\_INET을 sockaddr_in 구조체의 sin_family에는 AF_INET을 쓰는 것이 좋다고 한다.

예전에 socket() 함수를 설계할 때 전문가들은 하나의 프로토콜 안에서 서로 다른 주소체계를 쓸 수 있게 될것이라 생각하여 PF_INET을 만들었는데 현재의 IPV4에서도 아직까지 바뀐게 없어 그냥 그대로 사용되고 있다고 한다.

socket() 함수에서도 AF_INET을 써도 잘 작동되긴 하는데 어쨌튼 의미적으로는 다른 것이라고 하니 왠만하면 지켜가야할 듯.

참고자료 :: 네이버에 검색하면 부지기수로 있다;;;
