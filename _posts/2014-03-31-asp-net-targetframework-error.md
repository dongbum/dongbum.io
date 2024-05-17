---
title: ASP.NET targetFramework 특성 오류 대처방법
date: 2014-03-31T22:04:43+09:00
categories:
  - ASP.NET
---

```
ASP.NET MVC : 시작 오류 'targetFramework' 특성을 인식할 수 없습니다. 특성 이름은 대/소문자를 구분합니다.
```

라는 오류가 나오는 경우,

해당 application은 .net 4.0으로 작성되었으며, IIS에서 응용프로그램 풀을 4.0으로 설정하지 않아서 발생되는 에러..

IIS의 응용프로그램 풀을 4.0으로 설정 또는 해당 IIS 마우스 오른쪽 클릭 > 웹 사이트 관리 > 고급 설정 목록 중에서 '응용 프로그램 풀'을 'ASP.NET v4.0'으로 변경.

참고 : http://angeleyes.tistory.com/299
