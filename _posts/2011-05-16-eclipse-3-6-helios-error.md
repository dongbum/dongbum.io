---
title: Eclipse 3.6 Helios Error
date: 2011-05-16T13:28:06+09:00
categories:
  - Flash/Flex/ActionScript
  - Java/JSP
tags:
  - eclipse
  - Flash
  - Flash Builde
  - Helios
  - redmine
  - 플래시
  - 플래시빌더
---
플래시 빌더를 4.5 버전으로 새로깔고 이것저것 플러그인을 설치하던 중 예상치 못한 에러를 만났다.

![](/assets/images/eclipse-helios-error.png)

Redmine-connector를 인스톨하다가 난 오류인데 도저히 이 메시지를 보고 해결을 못하겠다. glassfish, jms라는 플러그인을 검색해봤으나 실패....

```
An error occurred while collecting items to be installed  
session context was:(profile=epp.package.java, phase=org.eclipse.equinox.internal.p2.engine.phases.Collect, operand=, action=).  
No repository found containing: osgi.bundle,javax.mail.glassfish,1.4.1.v201005082020  
No repository found containing: osgi.bundle,org.eclipse.equinox.log,1.2.100.v20100503  
No repository found containing: osgi.bundle,org.eclipse.net4j.jms.api,3.0.0.v20110215-1551
```

도대체 이 오류가 뭔질 모르겠다. 다른 플러그인을 깔면서도 오류 때문에 깔리질 않아서 Subclipse를 빼고는 제대로 플러그인을 쓸수가 없는 상태. 이클립스 3.6 헬리오스 버전이 문제가 좀 많은 것 같다. 구글링을 해보며 이런 저런 메시지들을 검색해본 결과 헬리오스 버전의 버그인 것도 있고... 하여간 골치 아프다.
