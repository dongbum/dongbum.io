---
title: Github와 연동 가능한 LGTM.com
date: 2019-10-01T12:19:24+09:00
categories:
  - C/C++
  - GIT
  - Python
tags:
  - code quality
  - github
  - lgtm
  - LGTM.com
---
Github에 돌아다니다보면 다음과 같은 뱃지를 볼 수 있는데,

![](/assets/images/github_scrot.png)

저기 있는 코드 퀄리티 뱃지가 무엇인지 찾아보니 LGTM.com이라는 서비스였다.

원래 LGTM은 Looks Good To Me의 약자로 깃허브에서 다른 사람의 코드를 보고 '아주 좋은 코드다.'라는 의미의 리플을 남길 때 쓰는 단어이다.

여튼 이 서비스는 **깃허브 레포지토리의 소스코드를 읽어들여서 소스코드의 품질을 체크하고 보안이슈나 위험한 부분을 체크해주는 서비스** 이다.

<https://lgtm.com> 에 가서 가입하고 깃허브 레포지토리를 연동해야한다. 연동 후 소스코드의 품질을 자동으로 측정하고 위험부분을 표시해준다.

cppcheck 등의 프로그램과 거의 동일하다. 다만 깃허브와 연동하기 편하게 되어있고, 온라인으로 알아서 작동한다는 것이 편리하다.

내 레포지토리에서는

![](/assets/images/lgtm-screenshot.png)

사용되지 않는 변수 등을 이렇게 지적해준다.

대략 C++, Python 등 메인 프로그래밍 언어들은 다 지원하므로 깃허브에 소스코드를 올려서 사용한다면 이러한 서비스를 같이 사용해보면 좋을 것 같다.
