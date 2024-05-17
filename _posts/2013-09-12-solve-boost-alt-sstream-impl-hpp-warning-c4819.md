---
title: Boost alt_sstream_impl.hpp warning C4819 에러 해결 방법
date: 2013-09-12T12:11:31+09:00
categories:
  - C/C++
tags:
  - alt_sstream_impl.hpp
  - boost
  - C
  - 부스트
---
열심히 작업하고 빌드를 하는데 계속 warning 메시지가 떴다.

내용은

```console
1>C:Libraryboost_1_51_0boost/format/alt_sstream_impl.hpp : warning C4819: 현재 코드 페이지(949)에서 표시할 수 없는 문자가 파일에 들어 있습니다. 데이터가 손실되지 않게 하려면 해당 파일을 유니코드 형식으로 저장하십시오. (..srcbusinessservicepbbmcacheMemberInfoManager.cpp)
```

빌드가 안되는 것도 아니고 프로그램에 오류가 있는 것도 아니지만 뭔가해서 이 메시지를 찾아보았다.

저 메시지를 더블클릭하면 부스트의 alt_sstream_impl.hpp 파일이 열리면서 파일 인코딩에 문제가 있다고 팝업창이 뜬다. 이 파일을 열고 한참 봤지만 문제가 없어보인다. 그냥 인코딩변환해버리면 저 에러메시지가 사라지겠지만 부스트 파일을 건드리는건 여간 기분이 찜찜한지라 구글에서 검색시작.

StackOverflow에서 다음의 글을 찾았다.

<http://stackoverflow.com/questions/10501634/warning-c4819-how-to-find-the-character-that-has-to-be-saved-in-unicode>

글 내용을 읽어보니 alt_sstream_impl.hpp 파일에 176번 라인의 주석이 문제였다. 에휴...

해결방법은 Notepad++로 이 파일을 열고 176번 라인의 주석을 모두 지워버리고 다시 빌드해보면 위 오류 메시지가 없어진다.
