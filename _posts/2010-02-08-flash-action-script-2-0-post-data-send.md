---
title: Falsh Action Script 2.0에서 POST 방식 데이터전달
date: 2010-02-08T06:47:44+09:00
categories:
  - Flash/Flex/ActionScript
tags:
  - Get
  - Post
  - 플래시
---
플래시에서 외부 파일과 통신할 때 쓰이는 방법 중 GET 방식에 이은 POST 방식.

애초 설계부터 GET 방식과 달리 데이터 전송에 더 목적이 있었기 때문에 여러가지 정보를 파라미터로 보낼 수 있다.

단, 이럴 때 LoadVars 객체를 두번 써야하는 번거로움이 생겨난다.

```actionscript
var data_1:LoadVars = new LoadVars(); // 객체 생성
var post_data:LoadVars = new LoadVars(); // 객체 생성

post_data.onLoad = function(success:Boolean) {
     if (success) {
          if (post_data.result == 1) {
               // 돌아온 데이터의 result 값이 1일 때 처리
          } else if (post_data.result == 2) {
               // 돌아온 데이터의 result 값이 2 일 때 처리
          } else {
               // 돌아온 데이터의 result 값이 그 이외일 때 처리
          }
     } else {
          // 데이터를 보내고 다시 되돌아온 수신이 오류일 경우 처리 루틴
     }
};

data_1.param1 = text1; // 넘길 데이터
data_2.param2 = test2; // 넘길 데이터
data_1.sendAndLoad("/post_data_test/test.jsp", post_data, 'POST'); // POST로 전송시작
```

GET 방식에 비해 LoadVars 객체를 한번 더 생성해야하는지라 코드가 약간 더 길어진다.

파라미터로 계속 지정해나가면 되서 데이터를 정렬하고 관리하는게 좀더 쉬워진다는 장점이 있고 GET과는 다르게 sendAndLoad 명령어를 쓴다는 점 정도만 기억하면 될듯.

특별히 어려운건 없지만 번거롭고 코드가 조금이라도 길어짐과 LoadVars를 두번 쓰다가 헷갈릴 수가 있어 난 사용을 잘 안하지만 프로그래머가 POST 데이터로 받고 싶다고 우긴다면 해줘야하므로 이 방법 역시 알고는 있어야할듯.
