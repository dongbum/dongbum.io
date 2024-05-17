---
title: TextField에서 htmlText 속성으로 쓸수 있는 html 태그들
date: 2009-09-29T15:20:03+09:00
categories:
  - Flash/Flex/ActionScript
tags:
  - Flash
---
일단 링크 주소. 찾느라 한참 고생했네 우씨. ㅡㅡ;;

<http://help.adobe.com/ko_KR/AS3LCR/Flash_10.0/flash/text/TextField.html#htmlText>

결론적으로는....

```
<a></a> :: 링크
<b></b> :: 볼드
<font></font> :: 폰트 설정
<u></u> :: 언더라인
<i></i> :: 이탤릭
<img> :: 이미지
<p></p> :: 문단
<span></span> :: CSS 스타일
<textformat></textformat> :: 텍스트포맷
```

다른건 그냥 뭐 무난무난. img 태그가 지원되는줄은 몰랐다. 신기하네. 그렇다면 혹시 텍스트박스 안에서 드래그 & 드랍도 되려나. ㅡㅡ;; 안될지도 머...

span이나 textformat을 쓰려면 좀 복잡한거 같은데... 잘만 쓰면 꽤 많은 기능을 구현 할 수 있을 것 같기도하다.

일단 이걸 가지고 웹에디터 모듈을 한번 작성해봐야할것 같다.
