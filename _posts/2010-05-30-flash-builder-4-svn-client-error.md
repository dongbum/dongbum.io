---
title: Flash Builder 4에서 SVN 클라이언트 오류 해결 방법
date: 2010-05-30T16:45:26+09:00
categories:
  - Flash/Flex/ActionScript
tags:
  - Flash Builder 4
  - JavaHL
---
맥부기가 수리 다녀오면서 OSX도 새로 깔고 Adobe CS5 패키지도 새로 설치했다.

Flash Builder 4를 켜고 Subclipse를 설치하고 레포지토리에서 체크아웃을 하려고하니 에러가 뜬다.

```
Failed to load JavaHL Library.
These are the errors that were encountered:
no libsvnjavahl-1 in java.library.path
no svnjavahl-1 in java.library.path
no svnjavahl in java.library.path
java.library.path = /usr/lib/jvm/java-6-sun-1.6.0.03/jre/lib/i386/client::/
usr/lib/jvm/java-6-sun-1.6.0.03/jre/lib/i386::/usr/lib/firefox:/usr/lib/
firefox/:/usr/java/packages/lib/i386:/lib:/usr/lib
```

대충 이런 비슷한 내용임. 대강 읽어보면 패쓰에 자바 라이브러리가 없다는 얘기 같다. JDK와 자바관련 패키지는 OSX에서 소프트웨어업데이트로 알아서 관리해주므로 안되어있을리가 없고....

여하튼 관련 에러메시지를 스크린샷으로 잡아놨는데 스크린샷 잃어버림;;

회사컴퓨터에서 Windows XP에 똑같이 CS5 마스터콜렉션으로 Flash Builder 4를 설치했을 때는 아무런 오류가 없었다. 왜 맥에서만 오류가 있을까 의아했음.

왜이러지...하면서 구글을 켜서 해당 오류메시지 첫째줄을 입력하고 검색 시작. 인터넷 여기 저기 둘러본 결과 JavaHL 라이브러리를 설치해야한다는 얘기가 있는데 왠지 아닌것 같아서 좀더 구글을 찾아보기로 함. 사실 라이브러리 파일을 찾아서 깐다는건 매우매우 귀찮기도하거니와 좀 검증되지 않은 패키지를 깐다는건 여간 찜찜하기 때문이다.

<http://entireboy.egloos.com/4201369>

결국 찾았다. 따로 라이브러리를 설치하지 않아도 됐고 위 블로그에 있는 내용대로 하면 해결된다. Subclipse 레포지토리에서 SVNKit 패키지를 설치하고 Preference에서 설정해주면 오류가 사라진다.

으... 내가 겪는 문제들은 어디선가 누군가 이미 다 겪은 것이라는 생각이 또 든다. 한편으로는 사람들이 이클립스 설치하고 환경설정하는 것 자체가 까다롭다고 하는 것도 이해가 간다. 설치에서부터 에러가 나기 시작하니 이거원...
