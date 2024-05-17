---
title: JAVA/JSP에서 reCAPTCHA 시스템 설치
date: 2012-12-30T16:40:07+09:00
categories:
  - Java/JSP
tags:
  - captcha
  - Java/JSP
  - JSP
  - reCAPTCHA
  - 스팸
---

![](/assets/images/smallCaptchaSpaceWithRoughAlpha-1.png)

최근에 게시판을 만들어놨는데 공개는 해놓지 않았다. 그냥 비공개로 올려놨는데 스팸봇들이 어찌나 설치대는지... 그래서 오늘은 captcha 시스템을 찾아보았다. 캡챠(이렇게 발음하는게 맞는지 모르겠다.)는 스팸봇들이 글을 함부로 업로드할 수 없도록 해주는 시스템이다.

Captcha의 경우에는 여러가지가 있는데 그중 요즘 가장 유명한 reCAPTCHA에 대해 찾아보기로 했다. 사실은 다른 캡챠 시스템은 찾아보기가 귀찮아서 그냥 들어본 기억이 이것밖에 없어서 그냥 이걸 쓰기로 결정했다. 쿨럭....

reCAPTCHA는 <http://www.google.com/recaptcha> 에서 확인할 수 있다. recaptcha를 사용하려면 인증키를 받아야하는데 이건 구글 계정으로 로그인해야 받을 수 있다.

각 사이트를 하나 추가할 때마다 그 사이트에 맞는 공개키와 개인키를 발급 받는다. 이 키는 나중에 프로그래밍에 쓰인다.

로그인한 다음 사이트를 추가하고 키를 발급 받았다면 이제 본격적으로 사이트에 reCAPTCHA 기능을 넣으면 된다. 여러가지 언어로 지원이 되는데 내 경우에는 JAVA/JSP로 사이트를 구성했으므로 그에 맞는 문서를 찾았다. JAVA/JSP에서 reCAPTCHA를 쓰는 경우에는 <https://developers.google.com/recaptcha/docs/java?hl=ko> 에서 도움말을 볼 수 있다.

설치하는 방법은 별로 어렵진 않다.

일단 <http://code.google.com/p/recaptcha/downloads/list?q=label:java-Latest> 에서 라이브러리 파일을 다운로드 받고 압축을 푼다음 jar 파일을 lib 폴더에 복사한다.

글 입력 페이지 부분에 1 항목에 Client Side 부분에 있는 코드를 입력하면 된다. public_key와 private_key 부분에는 아까 발급 받은 페이지의 공개키와 개인키를 복사해서 넣어주면 된다.

그리고 입력된 글을 처리하는 페이지나 서블릿에서는 Server Side 부분에 있는 코드를 입력한다. 역시 private_key 부분에는 아까 발급 받은 페이지의 개인키를 복사해서 넣는다.

여기까지가 끝이다. 정말 끝이다. 테스트를 몇번 해보니 잘 작동한다.

너무 쉽게 성공해서 허무했다. 어려울줄 알았는데... 이걸로 스팸봇이 얼마나 막아지려나 한번 기대해봐야겠다.
