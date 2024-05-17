---
title: Flash Action Script 2.0에서 GET 방식 데이터전달
date: 2010-02-08T06:12:37+09:00
categories:
  - Flash/Flex/ActionScript
tags:
  - Get
  - Post
  - 플래시
---
가끔 개발자들과 연동 작업을 하다보면 플래시에서 어떻게 데이터를 넘겨달라고 지정해줄때가 많은데 그중 GET 방식과 POST 방식이 제일 흔하다.

GET 방식은 쉽게 설명해서 URL에 데이터를 넣어서 보내는 방식.  
POST방식은 그렇지 않은;; 방식이다.

GET 방식으로 보내고 받을 때.  
LoadVars를 이용해 새 객체를 만들고 OBJECT.load(url) 형식으로 부르면 된다.

```actionscript
var object_name = new LoadVars();
object_name.onLoad = function(success:Boolean) {
     if (success) {
          // 로드가 성공했다면 수행할 동작
          trace(object_name.test_data)
     } else {
          // 로드가 실패했다면 수행할 동작
          trace("Faild")
     }
}
object_name.load("경로/파일명?변수명1=변수1&변수명2=변수2&..........')
```

GET 방식은 굉장히 유용하게 쓰이고 있다. 일단 URL 뒤에 변수명=변수 식으로만 계속 이어붙여주면 되기 떄문에 다루기도 쉽다. onLoad 명령은 통신을 한후 그 결과를 봐서 제대로 통신했는지를 알수 있다. 'object_name.변수명'을 이용해서 리턴된 값을 이용해 어떠한 다른 처리를 나누어 할 수도 있다. GET 방식은 애초부터 간단한 정보들만 넘기도록 만들어진 것이라 많은 정보를 처리할 때는 POST로 처리함이 옳지만 URL에 뒤에 변수명=변수 식으로 계속 붙이기만 하면 되니 사용하기가 편해서 굉장히 많이 쓰인다.

URL에 데이터들이 들어가므로 보안상 문제가 좀 있다. 아무리 플래시 내부에서 처리한다하지만 HTTP 패킷을 캡쳐하면 내용이 보인다. (하긴 그건 POST 방식도 매한가지이긴하지만.)

플래시에서 외부파일과 데이터를 주고 받을 때는 반드시 알아야할 방법 중 하나.
