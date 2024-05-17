---
title: 아파치에서 특정 도메인에 대한 페이지를 모두 포워딩하기
date: 2013-12-15T01:24:59+09:00
categories:
  - Linux
tags:
  - linux
  - 리눅스
  - 아파치
---
아파치 웹서버에서 가상호스트를 설정하여 운영하던 중에 특정 가상호스트로 들어오는 모든 요청을 특정도메인의 특정페이지로 넘길 때의 설정 방법이다.

가상호스트 설정 파일에서 다음의 내용을 입력한다.

```
<VirtualHost *:80>
 RewriteEngine On
 RewriteCond %{HTTP_HOST} ^(request.domain.com) [NC]
 RewriteRule ^(.*)$ http://target.domain.com/target_page.jsp [R,L]
</VirtualHost></pre>
```

지정된 도메인으로 오는 모든 요청이 지정된 페이지로 다 전환되어 버린다.
