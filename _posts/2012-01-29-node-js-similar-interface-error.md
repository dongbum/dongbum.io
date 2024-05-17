---
title: Node.js에서의 similar interface 오류 해결 방법
date: 2012-01-29T22:56:13+09:00
categories:
  - node.js
tags:
  - node.js
---
Node.js에서의 몇개의 테스트 코드를 만들어보았다.

웹서핑을 하며 본 몇개의 샘플 코드를 작성하고 실행해보며 신기해하던 중 오류를 만났다. 아니, 오류라기 보다는 경고 정도의 메시지라고 봐도 될 것 같다.

다음은 내가 입력한 코드다.

```javascript
var exec = require("child_process").exec;
var sys = require("sys");
var child;

child = exec("pwd", function(error, stdout, stderr) {
sys.puts(stdout);
});
```

코드의 내용은 pwd라는 명령어를 실행하고 결과를 반환하는 코드다. 코드를 모두 입력하고 실행하였더니 다음과 같은 결과가 나왔다.

```console
[viper9@내서버 ~]$ node command.js  
The "sys" module is now called "util". It should have a similar interface.  
/home/viper9

[viper9@내서버 ~]$
```

원했던대로 pwd 명령에 대한 결과는 나왔지만 위에 경고 메시지가 추가되었다. 사실 간단한 웹서버를 만들어서 시스템명령을 수행시키고 결과를 반환 받을 프로그램을 만들 예정이기 때문에 저 에러메시지가 추가되면 안되는 것이었다. 다시 확인해봐도 제대로 입력되었고 분명 Node.js 레퍼런스에도 있는 모듈인데 왜 경고가 뜨는지 알 수 없었다. sys 모듈이 util을 호출하고 이것은 비슷한 인터페이스라는 메시지여서 코드에 util 모듈을 추가해봤으나 결과는 같았다. 왜 그럴까 고민하다가 구글에서 저 경고 메시지를 넣고 검색 시작...

<http://groups.google.com/group/nodejs/browse_thread/thread/44ccefc8d9f4a5ac?pli=1> 에서 해결 방법을 찾을 수 있었다. sys 모듈을 제거하고 util 모듈로 변경하라는 얘기였다. 또한 인스턴스 모두를 util로 변경하면 된다였다. 그래서 sys 모듈 로드 부분을 util 모듈로 변경하고 sys 모듈을 사용하는 부분을 util 모듈을 사용하도록 변경했더니 원했던대로 결과가 나왔다.

sys 모듈과 util 모듈에 대해 좀 더 레퍼런스를 읽어봐야 할 것 같다.
