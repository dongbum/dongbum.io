---
title: '죠리퐁세기 - ActionScript 버전'
date: 2005-12-13T08:26:00+09:00
categories:
  - Flash/Flex/ActionScript
---
과연 죠리퐁 한봉지 안에는 몇개의 죠리퐁이 들어있을까...?

2003년도에 KLDP에 시작된 죠리퐁세기에 관한 글타래.

[글타래 보러 가자~~~](http://bbs.kldp.org/viewtopic.php?t=26098)

'그렇게 폐인이면 죠리퐁이나 세어봐라'식으로 시작된 이 글타래는 KLDP분들의 각종 프로그래밍 언어로 만들어졌다. 이중 내가 생각한 가장 획기적인 방법은 레고 마인드스톰을 연결해서 센서를 이용해 세자는 것 같다.

여튼 오랜만에 글을 읽어보니 재미있기도 해서 나도 살짝 만들어봤다.

다음은 내가 생각한 죠리퐁세기의 Flash ActionScript 버전이다. 실제로 가능할지는 모르겠다;; 다른 프로그래밍 언어에 비해서 스크립트 언어라 죠리퐁의 개수를 세는 센서를 연동하거나 하는건 무리일것 같다. 에... 아마 Java나 C, PHP 등을 연동한다면 가능할지도 모르겠다.

```actionscript
//
// JollyPong count scripts
// Made with Macromedia FlashMX 2004
// Copyright(c) 2005. Dong-bum, Kim (http://www.dongbum.com)
//
// 버튼을 클릭하면 빈 무비클립을 만들어 조리퐁을 로드하여 total_jolly에 넣는다. 타임라인의 한프레임당 죠리퐁 한개씩.
// 죠리퐁을 로드
// 조리퐁이 모두 로드 되지 않았다면 에러메시지 출력
// 조리퐁이 모두 로드 되었다면 jolly가 total_jolly의 모든 프레임과 같아질 때까지 하나씩 증가시키며 카운트
// jolly와 total_jolly가 같다면 카운트가 끝난 것이므로 output 출력

on (release) {
  this.createEmptyMovieClip("total_jolly", this.getNextHighestDepth());
  loadVariables("조리퐁", total_jolly);
  if (total_jolly.done == undefined) {
    trace("아직 죠리퐁이 모두 로딩되지 않았습니다.");
  } else {
    var jolly:Number = 0;
    for (var i = 1; i == total_jolly._totalframes; i++) {
    jolly += i;
  }

  trace("조리퐁의 갯수는 "+jolly+" 입니다.");
  }
}
```

음...... 문제는 두개가 붙어있는 죠리퐁을 계산할수 없다는 것. (두개 이상이 붙은 죠리퐁은 개수를 나누는 코드가 추가되어야할 것 같다.

간단하게 뭐 이런식이면 되지 않을까... 싶은데 여튼 더 연구해봐야겠다. 🙂
