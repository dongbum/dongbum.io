---
title: CentOS 8 지원 종료
date: 2021-01-01 01:44:01
categories:
  - Linux
tags:
  - Linux
  - CentOS
---

![](/assets/images/centos_logo.png)

### 뜬금 없는 지원 종료 발표

내 경우에는 CentOS 7에서 8로 넘어간게 불과 1년도 안된 것 같은데... 얼마전 CentOS 8 의 지원 종료를 발표했다. 약간의 배신감도 느껴진다.

CentOS 8은 CentOS 8 Stream으로 변경된다.

CentOS 8 Stream 의 의미는, 기존에는 RHEL이 만들어지고 그것을 카피한 CentOS가 만들어졌기 때문에 사실상 RHEL의 안정성을 그대로 가져온다고 생각하면 되었는데 이제는 그 반대가 되어서 CentOS 가 만들어지고 여기서 테스트를 진행한 다음 RHEL로 넘어가게 된다.

RHEL의 테스트베드가 되는 셈.

CentOS 8의 지원은 2021년 12월 31일에 종료된다.

### 선택할 수 있는 대안

CentOS 8 사용자는 세가지 방향이 있는데,
1. 지원과 업데이트가 끊긴 CentOS 8 을 그냥 쓰던지
2. CentOS 8 Stream 으로 전환하여 사용하던지
3. CentOS 7 로 변경하던지
4. 아니면 아예 다른 리눅스로 변경하던지

네가지 방안이 있다. CentOS 7 은 2024년 6월 30일까지 지원한다고 하니 3년 더 버틸 시간이 주어졌다. 그런데 3년 후에는 어차피 CentOS 7도 마찬가지로 지원이 끊기고 업데이트가 끊기면 이 역시 문제가 발생할 소지가 크다.

테스트베드가 되어도 된다고 생각하는 사람들은 CentOS 8 Stream 으로 변경하던지 아니면 Ubuntu Server 로 전환하던지 하는게 낫지 않을까 싶다.

장기적으로 봤을 때에는 어차피 CentOS 9는 나오지 않을 것이고 CentOS 7은 없어질 것이며 CentOS 8에 대한 지원이 계속 줄어들지 않을까 생각되어서 아예 우분투서버로 넘어가는 것을 고려해봐야할 것 같다.

### 참고자료

1. <http://www.opennaru.com/linux/centos-%EC%A2%85%EB%A3%8C/>
