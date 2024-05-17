---
title: Valid URL 정규식
date: 2009-08-17T05:05:54+09:00
categories:
  - Flash/Flex/ActionScript
---

```actionscript
var url : String = http://www.google.co.kr/;
var regex:RegExp = /^http(s)?://((d+.d+.d+.d+)|((\[w-]+.)+([a-z,A-Z\]\[w-\]\*)))(:\[1-9\]\[0-9\]\*)?(/([w-./:%+@&=]+[w- ./?:%+@&=]\*)?)?(#(.\*))?$/i;  
trace(regex.test(url));
```

외국 플래시미디어서버 사이트 돌아다니다가 발견.

그런데 생각보다 그다지 정확하지는 않은듯....
