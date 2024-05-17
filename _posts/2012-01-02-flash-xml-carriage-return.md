---
title: XML에서 캐리지리턴(엔터) 문제
date: 2012-01-02T20:56:52+09:00
categories:
  - Flash/Flex/ActionScript
tags:
  - ActionScript
  - Flash
  - XML
  - 액션스크립트
  - 플래시
---
플래시에서 다음과 같은 코드를 입력했을 때 캐리지리턴(=엔터)가 한번이 아닌 두번이 입력되는 현상이 발생한다.

```actionscript
var xml:XML =

test11
줄바꿈


trace("=========================================");
trace(xml.child("title"));
trace("=========================================");
trace(xml.child("title").split("r").join(""));
trace("=========================================");
```

원래 XML의 내용대로라면 한줄 내려서 입력되어야하지만 실제적으로는 빈 행이 두줄이 입력된다는 것. 어도비의 레퍼런스에서 XML, XMLList 부분을 아무리 찾아봐도 이에 대한 내용이 없다. 플래시의 버그인가..? 하는 생각을 하면서 구글신에게 물어본 결과 다음과 같은 글을 찾았다.

<http://stackoverflow.com/questions/570656/as3-xml-and-line-spacing-problem>

글 내용인 즉슨, line break에서 Carriage Return과 Line Feed가 같이 입력되므로 rn 이렇게 입력된다는 것이었다. 그러므로 r로 입력된 부분을 찾아서 지우라는 말. String 클래스의 split 메서드를 써서 했다길래 나도 split 메서드로 해결 완료. 잘 된다. split 보다는 replace 메서드를 쓰는게 더 나을듯하다.

밑에 리플들을 보면 아마 서버 플랫폼에 따라 차이가 있다는 것 같은데 이것은 내가 윈도우서버를 가지고 있지 않으므로 패쓰.
