---
title: https 접근 문제 해결방법
date: 2009-10-14T13:59:19+09:00
categories:
  - Flash/Flex/ActionScript
  - Programming
tags:
  - Flash
  - https
  - SSL
  - 플래시
---

플래시에서 SSL이 적용된 경로로 접근하려할때 접근이 되지 않는데 이럴 경우 다음 코드를 HTML에 삽입함으로써 해결할 수 있다.

```html
<meta http-equiv="Expires" content="-1" />
<meta http-equiv="Pragma" content="no-cache" />
<meta http-equiv="Cache-Control" content="no-store,max-age=0,must-revalidate " />
```

안타까운건 이 코드를 이용해 '웹'에서는 해결할 수 있지만 AIR에서는 여전히 https로의 접근이 되지 않는다는 것... 언제쯤 이 문제가 해결될라나 모르겠다.
