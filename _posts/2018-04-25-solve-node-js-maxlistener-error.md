---
title: Node.js MaxListener 에러 수정
date: 2018-04-25T16:41:43+09:00
categories:
  - node.js
tags:
  - node.js
---
웹소켓 테스트 중 마주친 에러.

대략 코드상으로는 별 문제가 없는데 실행시 이러한 에러가 나왔다.

```
(node:5828) MaxListenersExceededWarning: Possible EventEmitter memory leak detected. 11 close listeners added. Use emitter.setMaxListeners() to increase limit
```

해결방법은

  * <https://stackoverflow.com/questions/8313628/node-js-request-how-to-emitter-setmaxlisteners>
  * <https://stackoverflow.com/questions/9768444/possible-eventemitter-memory-leak-detected>

를 참고하여 해결.

코드 중간에 다음과 같은 코드를 삽입한다.

```javascript
require('events').EventEmitter.prototype._maxListeners = 100;
```

그런데 이 코드를 넣으면 이벤트 전반에 대한 리스너 갯수를 변경하는지라... 글로벌하게 변경해도 되나 모르겠네. 웹소켓 객체에다가만 적용하는 방법을 더 찾아봐야할 것 같다.
