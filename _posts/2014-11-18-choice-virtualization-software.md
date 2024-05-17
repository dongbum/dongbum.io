---
title: 가상화 하이퍼바이저의 선택
date: 2014-11-18T13:22:10+09:00
categories:
  - Virtualization
tags:
  - ESXi
  - Hyper-V
  - Virtualization
  - VMWare
  - Xen
  - XenProject
  - XenServer
  - 가상화
---
서버를 가상화하기로 생각하고 처음 생각한건 가상화 하이퍼바이저의 선택이었다. ([위키피디아 : 하이퍼바이저](http://ko.wikipedia.org/wiki/%ED%95%98%EC%9D%B4%ED%8D%BC%EB%B0%94%EC%9D%B4%EC%A0%80))

하이퍼바이저의 선택이 잘못되었을 때 나중에 바꾼다는건 굉장히 어렵고 복잡하므로 처음 선택이 중요하다.

내가 회사에서 사용하고 있던 것은 VMWare의 ESXi 5.0이었고 이번에 바꾸기 위해 몇가지 하이퍼바이저를 찾아보고 조사해보았다.

---

### Microsoft - Hyper-V

![](/assets/images/mshv-overview.png)

마이크로소프트의 하이퍼브이 시스템.(<http://www.microsoft.com/ko-kr/server-cloud/solutions/virtualization.aspx>)

![](/assets/images/HyperV_Homepage.jpg)

장점은 익숙한 윈도우GUI를 그대로 이용할 수 있다는 것. 시중에 이미 [Hyper-V를 다루는 기술](http://www.yes24.com/24/Goods/14832227?Acode=101)이라는 하이퍼브이에 관련된 책이 나와있기 때문에 참고하기 쉽다는 점이 있었다. 실제로 교보문고에 가서 이 책을 잠시 읽어봤는데 내용도 상당히 괜찮았다.

하지만 단점도 있다. 하이퍼브이는 하이퍼브이를 지원하는 운영체제만 공식지원하고 있다. (참고 : <http://technet.microsoft.com/ko-kr/library/cc794868(v=ws.10).aspx>) 게스트 운영체제가 이를 지원하지 않으면 사용상 애로사항이 생길 우려가 보였다. 또한, 사실상 콘솔모드로 작동하는 Windows Hyper-V를 쓰지 않으면 Windows 서버 운영체제를 구입해야한다는 경제적인 문제도 고려해야했다.

---

### Xen Project - XenServer

![](/assets/images/xen_project_logo_384x157.png)

Citrix사에서 시작된 Xen Project의 XenServer. (<http://www.xenproject.org>)

![](/assets/images/Xen_Homepage.jpg)

XenServer는 국내외에서 많이 사용되고 있다. 가장 유명한 아마존 AWS도 Xen을 사용하고 있고 KT의 ucloud시스템도 Xen을 이용하고 있다고 한다. 반가상화 시스템에서 오는 성능적인 장점이 굉장히 크다고 한다.

사실 ESXi를 설치해보다가 Xen도 한번 설치해본 것인데, 기본적인 설치방법이라던가 설치한 이후 화면에 나오는 것은 ESXi와 별반 다르지 않다. 단지 ESXi는 노랑색 화면이 나오고 Xen은 옅은 회색 화면으로 나온다는 정도?

장점은 전가상화가 아닌 반가상화이기 때문에 성능이 매우 좋다는 점. 여러 업체가 쓰고 있다는 사실만으로 믿음직하다는 점이다. 그럼에도 내가 Xen으로 가지 않은 이유는, 사용상 너무 불편하다. 특히 ESXi와 비교해서 너무 불편했는데, 가상운영체제를 설치하려고 할 때 ESXi는 ISO 파일을 그냥 업로드한 다음 마운트 해주면 그만이지만, Xen은 ISO파일을 올리고 그것을 공유디렉토리로 마운트 하는 식으로만 접근이 가능했다. 왜 이런식의 접근을 요구하는지 잘 모르겠으나 어쩄튼 사용방법이 너무 번거롭고 귀찮아서 제외하기로 결정.

---

### VMWare - ESXi

![](/assets/images/esxi-dedicated-server-icon.png)

VMWare의 ESXi. (<http://www.vmware.com/kr/products/vsphere-hypervisor>)

![](/assets/images/VMWare_Homepage.jpg)

가상화 체제에서 아직까지는 최고로 유명한 ESXi. 사실 내가 계속 사용해온 것이기도 하고 위 두가지와 비교했을 때 사용상 제일 편리했다.

장점은 편리한 사용방법, 인터넷에 자료가 많다는 점. 전세계적으로도 많이 쓰이고 있다는 점, 그리고 전가상화 시스템이기 때문에 위 두 운영체제와는 달리 어떤 운영체제도 지원한다는 점이 제일 좋았다. 사실 어떤 운영체제든 다 지원한다고는 할 수 없겠으나 vSphere Client에서 지원하는 운영체제는 현존하는 대부분의 메이저 운영체제들은 다 지원한다고 봐도 무방하다. 그리고 ESXi는 무료로 배포되고 있다. 다만, 라이센스는 무료로 발급 받을 수 있으나 사용상의 제약조건이 존재한다. 단점은 전가상화로 인해 성능은 약간 낮다는 점이 있었다.

---

### 그래서 결국 무엇을 선택했는가.

결론적으로는 VMWare ESXi를 선택했다. 세가지 비교 결과 성능은 약간 낮을 수 있지만, 일단 사용상으로 불편하다면 계속 사용하지 않게 될 수 밖에 없었다. 더군다나 내 서버는 서비스용 서버라기 보다는 공부용 성격이 강하기 때문에 이런 저런 운영체제를 자유자재로 설치했다 지웠다 하게 되는데 Xen의 경우는 이게 너무나 불편해서 제외하게 되었다. 만약 성능을 중시한다면 Xen이 가장 좋은 선택이 아닐까 싶다. 하여튼 난 Hyper-V와 ESXi 중에 선택을 해야했는데 Hyper-V는 공식적으로 지원하는 운영체제가 제한적이었다. 사실 공식지원을 안하는 것이지 설치 자체는 잘 될 것 같지만 그래도 일단 기분이 다르니까. 만약 윈도우를 못 벗어나겠다거나 윈도우만 써야겠다, 난 MS가 신뢰가 간다면 Hyper-V를 선택해야할 것 같다.

결국에는 손에 제일 익었고 내가 사용할 상황에 가장 적절한 ESXi를 선택하고 지금도 잘 사용하고 있지만 나중에는 한번 Xen이나 Hyper-V도 설치해서 살펴봐야할 것 같다.
