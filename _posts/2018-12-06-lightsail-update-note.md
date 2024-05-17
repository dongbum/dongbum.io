---
title: Lightsail 을 사용한 이후 그동안의 업데이트와 변화
date: 2018-12-06T21:51:13+09:00
categories:
  - AWS
  - Lightsail
tags:
  - Amazon
  - AWS
  - EC2
  - Lightsail
  - 가상서버
  - 라이트세일
  - 라이트셰일
  - 아마존
  - 클라우드
---
AWS를 사용하기 시작한지도 꽤 된 것 같다. 이 서비스가 나온지 얼마 안되서부터 사용하기 시작했고 아쉬운 점이 많았는데 최근에 몇가지 기능들이 추가되었다.

### 리전간 스냅샷 복사 기능 추가

오늘 알게된 내용인데 리전간 스냅샷 복사 기능이 드디어 추가되었다.

일본 리전에서 인스턴스를 만들어 쓰다가 몇달전 서울 리전으로 이동하려고 했을 때, 이 스냅샷 복사 기능이 아직 구현되어 있지 않았었다. 그래서 서울 리전에 인스턴스를 새로 만들어서 데이터를 수동으로 옮겼다. 그때 이 기능이 없어서 아쉬웠는데 이제서야 생겼네.

![](/assets/images/lightsail-snapshot.png)

스냅샷을 생성한 후 메뉴를 보면 다른 리전에 복사라는 메뉴가 생겼다. 그러고보니 Amazon EC2로 내보내기라는 메뉴도 생겼다...? 라이트세일에서 운영하던 인스턴스를 그대로 EC2로 가져갈 수 있다는 의미인건가...!?

![](/assets/images/lightsail-snapshot-regeon.png)

스냅샷 복사를 선택하면 리전은 어디로도 이동가능하다.

### 데이터베이스 기능 추가

데이터베이스를 별도의 인스턴스로 추가시켜주는 기능이다. 내가 라이트세일을 처음 이용할 때는 이런 기능이 없었는데 얼마전 추가되었다. 사실 라이트세일을 이용하는 유저라면 기본적으로 자기 서버에 데이터베이스를 설치해서 쓸줄 알것이라 생각되므로 이런 기능이 굳이 필요한지는 잘 모르겠다.

![](/assets/images/lightsail-database-mysql.png)

현재는 MySQL만 지원한다.

![](/assets/images/lightsail-database-plan.png)

가격은 월 15달러.

![](/assets/images/lightsail-database-plan-ha.png)

고가용성 옵션도 있는데 가격은 두배!

### 태그 기능 추가

어떤 인스턴스가 어떤 서비스를 담당하고 있는지 태그 기능으로 묶을 수 있다.

<https://aws.amazon.com/ko/about-aws/whats-new/2018/11/amazon-lightsail-now-supports-resource-tagging>

### EC2 업그레이드

lightsail에서 사용하던 인스턴스를 그대로 EC2로 가지고 갈 수 있다. 서비스가 크게 늘어나거나 하면 EC2로 가져갈 수 있게 만들어준듯. 한편으로는 라이트세일이 미끼용이기도하고. ㅎㅎ

<https://aws.amazon.com/ko/about-aws/whats-new/2018/11/amazon-lightsail-now-provides-an-upgrade-path-to-ec2>
