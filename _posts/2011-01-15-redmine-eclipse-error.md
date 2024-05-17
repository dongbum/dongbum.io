---
title: Redmine Eclipse 연동 삽질기
date: 2011-01-15T09:21:49+09:00
categories:
  - Java/JSP
tags:
  - eclipse
  - Flash Builder 4
  - redmine
  - 레드마인
  - 이클립스
---
서버에 redmine을 정말 어렵게 어렵게... 진짜 며칠간 개삽질해가며 설치하고... 생각해보니 trac은 이클립스 플러그인이 있어서 좀 나았는데 레드마인은 없었나 한번 찾아봤다. 역시나 있다. 참조한 글은 <http://rcnboys.blog.me/20119875598> 이 글이다. 플래시빌더를 켜고 Install New Software 클릭. 저장소를 추가하고 경로는 <http://redmin-mylyncon.sourceforge.net/update-site/N/> 로 입력한다. 하나 웃긴건 레드마인은 redmine 이라고 쓰던데 저 URL 보면 왜 remin 이라고 써놨을래나....

여튼 저장소를 추가하니 Pending 과정을 거치고 소프트웨어 선택이 나온다. 뭐 레드마인 커넥터 딱 하나 밖에 없으니 걍 선택하고 인스톨 시도.

안된다. ㅡㅡ;;

무슨 eclipse의 core runtime이 없다고 나온다. 다시 이클립스 갈릴레오쪽 저장소를 보니 이건 뭐 너무 많다. 프로그램이 한두개도 아니고... type filter에 'core'로 검색하니 Programming Tools 카테고리에 Core 어쩌구 들어간게 있어서 일단 이거 설치. (이 과정은 스크린샷으로 못 남김;;)

설치가 다 되었길래 부푼 맘을 안고! 다시 시도했다.

안된다.

에러 메시지를 읽어보니 MyLyn Connector가 없다고 한다. subversion 저장소에서 찾아 MyLyn 커넥터를 설치했다.

자 이제 다 깔았으니 다 되겠지~~~ 다시 시도했다.

안된다..

에러 메시지는...
```
Cannot complete the install because one or more required items could not be found.

Software being installed: Mylyn Connector: Redmine 0.1.0.201101130831 (net.sf.redmine_mylyn.feature.feature.group 0.1.0.201101130831)

Missing requirement: Mylyn Connector: Redmine 0.1.0.201101130831 (net.sf.redmine_mylyn.feature.feature.group 0.1.0.201101130831) requires 'org.eclipse.core.runtime 3.6.0' but it could not be found
```

subversion 에 있는 MyLyn 커넥터 얘기하는게 아니었나보다... 다시 인터넷을 뒤져서 MyLyn 커넥터 위치를 찾아봤다.

<http://download.eclipse.org/tools/mylyn/update/e3.4>

입력하고 찾아보면 MyLyn Intergration이 없다고 한다....

**2011년 01월 24일 추가.**

위에 쓴거 말고도 벼르이 별 삽질을 다 해봤는데 안되길래 마지막으로 kldp.org에 질문글을 올렸다.

결론은... 이클립스 3.5 버전에서는 불가능하다는거다. 3.6 이상에서만 된다고 한다. (플래시빌더는 이클립스 3.5 버전) 이클립스 3.5에서 설치하려면 옛날 버전 레드마인커넥터를 써야하는데 이건 또 최신 레드마인을 지원 안하고 0.9까지만 지원한덴다. (내 서버에 깔린 레드마인은 1.1대 버전)

결론은... 못.쓴.다. ㅡㅡ;; 며칠간의 삽질을 했다. 에휴...

그냥 다시 trac으로 돌아갈까? 라는 생각도 들고 Nforge를 써볼까 하는 생각도 들었는데 그래도 레드마인을 더 유지해보기로 했다.
