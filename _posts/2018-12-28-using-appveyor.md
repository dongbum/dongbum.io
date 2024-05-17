---
title: AppVeyor 사용기
date: 2018-12-28T13:34:31+09:00
categories:
  - boost
  - C/C++
  - Database/SQL
  - GameServer
---
![](/assets/images/appveyor-card.png)

Travis-CI 에서 윈도우 빌드가 되지 않아 고민하던 중...

새로운 CI 서비스를 찾았다. AppVeyor인데 앱베요인지 앱비요인지 앱베이요인지 뭐라고 읽어야할지 잘 모르겠다. 이 서비스는 Travis-CI와 같은 리눅스 빌드 외에도 윈도우에서 VisualStudio를 통한 빌드를 지원한다.

Appveyor는 Travis-CI처럼 yml 파일로도 설정해줄 수 있지만 웹사이트에서 바로 설정을 수정할 수 있기 때문에 아무래도 사용하기가 많이 편리했다.

Boost 라이브러리는 프로젝트 속성에 절대경로로 지정하던 것을 윈도우 시스템 환경변수로 뺐고, appveyor 에서는 프로젝트 설정에 환경변수로 넣어주었다.

MySQL 커넥터 라이브러리에서도 충돌이 생겼는데, appveyor 에는 MySQL ODBC 드라이버만 지원하고 있었다. 나는 리눅스에서도 빌드되게 하려고 따로 MySQL-C++-Connector를 사용하고 있다. 일단 이 라이브러리를 솔루션에 포함시키는 것으로 해결했다. 차후에는 ODBC 버전까지 같이 지원하도록 만드는 것이 나을 것 같다.

boost와 mysql을 해결하고 json과 jemalloc 라이브러리가 먼저 빌드되고 그다음 유틸리티 라이브러리, 네트워크 라이브러리 순서로 빌드하기 위해 솔루션 설정에서 프로젝트 종속성을 설정해주었다.

다시 빌드하자....

![](/assets/images/appveyor-build-success.png)

게임서버, 브릿지서버 둘다 빌드 성공.

확실히 GCC나 G++를 이용하는 빌드보다는 훨씬 쉬운 느낌이다.
