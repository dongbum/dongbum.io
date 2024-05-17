---
title: JAVA의 자료형
date: 2013-10-25T23:41:54+09:00
categories:
  - Java/JSP
tags:
  - Java/JSP
  - JSP
  - unsigned
  - 자료
  - 자바
---
서버와의 통신을 위해서 프로그램을 만들 일이 있어서 이것저것 기초작업을 시작해보다가 의문이 있어서 자료형에 대해 찾아보았다.

C++는 char, short, int, int64 등의 자료형과 여기에 대한 unsigned형들, C#에서는 byte, int16, in32, int64 등의 자료형들을 사용해왔다. 이클립스에서 jsp 파일을 작성하다보니 unsigned형에 대해 코드힌트가 나오는 것이 없었다. 그래서 인터넷을 검색해보니 몇가지 자료가 나왔다.

  * char가 2바이트의 크기를 가진다는 것.
  * 모든 자료형에 부호값을 가지기 때문에 unsigned라는 자료형이 따로 존재하지 않는다.

이 두가지에 가장 많은 차이가 있는 것 같다.

이 문서를 읽어보면 좋을듯하다.

<http://www.tipssoft.com/bulletin/board.php?bo_table=FAQ&page=11&wr_id=788>

자바에 대해 많이 익숙해졌다고 생각했는데 이런 중요한 차이점이 있다는 것을 이제야 알게되었다.
