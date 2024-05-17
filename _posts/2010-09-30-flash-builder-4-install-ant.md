---
title: Flash Builder 4에서 ANT 설치하기
date: 2010-09-30T00:06:16+09:00
categories:
  - Flash/Flex/ActionScript
  - Java/JSP
tags:
  - ant
  - eclipse
  - FB4
  - Flash Builder 4
  - JDT
  - RSE
  - 이클립스
---
플래시 작업을 하면 테스트 서버에 올리고 실제 서버에 올리고 파일 갱신 URL을 호출하고 서비스 URL을 열고.... 이 많은 노가다를 하기 싫어서 자동화하기 위해 Flash Builder 4에서 ANT를 쓰고 싶어 여기저기 검색해봤다.

결론은 **Flash Builder 4에는 ANT가 기본적으로 포함되어 있지 않더라.** 이클립스 기본툴인디... 어도비는 반성해야함.

JDT를 설치하면 된다길래 여기저기 검색.

[검쉰님의 블로그에 있는 글](http://blog.flashplatform.kr/213)을 참조로 따라서 설치해봤다.

검쉰님이 쓰신 것처럼 초기 스타트 화면이 바뀌어버리더라. 물론 그 글에 있는 리플처럼 인터페이스가 아예 한글로 바뀌어버리는 사태가;;; 끙...;;

결국은 Flash Builder 4를 다 지워버리고 새로 깔았다;;

그리고 다시 나혼자 여기저기 검색해가며 설치.

이렇게 하면 ANT만 살포시 설치 가능하다.

1. 소프트웨어 업데이트에서 <http://download.eclipse.org/releases/galileo> 입력하고 레포지토리 추가.</span>
2. 리스트중 Programming Language에서 Eclipse Java Development Tools를 선택
3. 다운로드 & 설치 과정 진행. (다행히도 다른 프로그램을 또 설치하라는 의존성 에러는 나오지 않았다.)
4. 이클립스 재시작.

이렇게 하고 나면...

메뉴에서 Windows -> Other View를 실행하면 ANT가 설치되어 있다.

휴. 겨우 깔았다. 이 다음번은 RSE 플러그인을 설치하면 될듯. 다음은 ANT 스크립트 작성해서 테스트해봐야겠다.

이런 노가다 안하게 어도비는 다음부터 ANT 같은 툴은 좀 기본으로 포함해주시길. 젭알.

**추가.**

이 과정을 거치고 나니 RSE도 제대로 깔린다. CDT라는 프로그램을 먼저 깔라고 난리쳤던 RSE가 그냥 깔려버렸다. 참 허무하네... 이제 이클립스에서도 FTP에 바로 접속 가능한건가... 더 테스트해봐야겠다.
