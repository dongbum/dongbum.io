---
title: onReleaseOutside in AS3
date: 2009-10-23T08:50:00+09:00
categories:
  - Flash/Flex/ActionScript
---
한게임에 들어가는 동영상플레이어 만들다가 on(releaseOutside) 를 대체할만한 이벤트가 없어서 고민하다가 걍 내버려뒀더니 한게임 플래시팀에서 버그 있다고 연락왔다. 검색엔진을 뒤진 끝에 찾아낸 해결책.

ActionScript 3.0에서 onReleaseOutside를 대체할 만한 이벤트가 없다. 그래서 MOUSE_DOWN 이벤트가 발생할 때, stage에 MOUSE_UP 이벤트를 추가했다가, MOUSE_UP 이벤트 핸들러에서 다시 MOUSE_UP를 제거시키는 방법을 사용해야 한다.

```actionscript
public function Test(){ // Add MOUSE_DOWN event to mc
        mc.addEventListener(MouseEvent.MOUSE_DOWN, rotateDragStart);
}

private function rotateDragStart(evt:MouseEvent):void{
        // Start drag
        // Add ENTER_FRAME event to mc
        mc.addEventListener(Event.ENTER_FRAME, rotateDragging);
        // Add MOUSE_UP event to stage
        stage.addEventListener(MouseEvent.MOUSE_UP, rotateDragStop);
}

private function rotateDragStop(evt:MouseEvent):void{
        // Stop drag
        // Remove ENTER_FRAME event from mc
        mc.removeEventListener(Event.ENTER_FRAME, rotateDragging);
        // Remove MOUSE_UP event from stage
        stage.removeEventListener(MouseEvent.MOUSE_UP, rotateDragStop);
}

private function rotateDragging(evt:Event):void{
        // Excute while dragging
}
```

출처 :: <http://hangunsworld.com/blog/category/flash/as3/page/10>
