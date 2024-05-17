---
title: StageDisplayState.FULL_SCREEN이 먹지 않을 때
date: 2010-10-11T04:17:10+09:00
categories:
  - Flash/Flex/ActionScript
---
잘 동작하던 플래시 파일에서 화면 최대화 버튼이 먹질 않았다.

```actionscript
stage.displayState = StageDisplayState.FULL_SCREEN;
```

이 코드가 먹지 않는 경우.

플래시를 임베디드 하는 object나 embed 태그에 다음 설정을 추가해줘야한다.

```html
<param name="allowFullScreen" value="true" />
```

이에 대한 내용.

> To enable full-screen mode, developers must add a new `<object>` and `<embed>` tag parameter, `allowFullScreen`, to their HTML. This parameter defaults to `false`, or not allowing full screen. To allow full-screen, developers must set `allowFullScreen` to `true` in their `<object>`/`<embed>` tags.

어도비의 문서 <http://www.adobe.com/devnet/flashplayer/articles/full_screen_mode.html> 를 읽어보면 된다.
