---
title: FlashDevelop에서 ActionScript 3.0 도움말 설정
date: 2009-10-27T14:24:34+09:00
categories:
  - Flash/Flex/ActionScript
tags:
  - FlashDevelop
---

플래시디벨롭. 거의 다 좋은데 SVN을 지원하지 않는다는점하고 영문판이라 좀 불편할 때가 있다. 가끔 레퍼런스를 찾아봐야하는데 단어에서 F1을 누르면 구글에다가 검색어를 뿌려서 페이지를 보여준다.

검색하며 외국 사이트 돌아다녀보니 다른 사람들도 나랑 똑같이 느끼나보다.

아무튼.

FlashDevelop - Tools - Program Settings - AS3Context - Documentation Command Line 을 다음과 같이 설정한다.

```
http://help.adobe.com/ko_KR/AS3LCR/Flash_10.0/$(ItmTypPkgNamePath).html#$(ItmName)
```

이제 단어에서 F1을 누르면 한글 어도비 레퍼런스로 바로 연결됨.
