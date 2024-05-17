---
title: DROWN Attack 취약점 방지용 SSL 설정
date: 2018-09-28T13:27:43+09:00
categories:
  - Linux
tags:
  - nginx
  - SSL
  - 서버
  - 엔진엑스
---
https://www.ssllabs.com 에서 SSL 테스트를 해봤는데 DROWN 취약점이 있다고 F 등급을 먹인다.

DROWN 공격 취약점에 대한 내용

  * https://sarc.io/index.php/httpd/581-openssl-suites
  * http://blog.alyac.co.kr/554

줄여서 얘기하자면, SSL v1, v2, v3와 TLS 1.0은 사용하지 않도록 하는 것이 목표이다.

그렇게 설정했는데도 아무리해도 되질 않았다.

  * https://serverfault.com/questions/704376/disable-tls-1-0-in-nginx

에서 답을 찾았다. default_server 설정이 있어야 한다는 것.

nginx virtualhost 설정에서

```nginx
server {
        listen                  443 ssl default_server;
        root                    /opt/bitnami/apps/blog.dongbumkim.com/htdocs/;
        server_name             blog.dongbumkim.com;
        ssl_certificate         /etc/letsencrypt/live/blog.dongbumkim.com/fullchain.pem;
        ssl_certificate_key     /etc/letsencrypt/live/blog.dongbumkim.com/privkey.pem;
        include                 "/opt/bitnami/nginx/conf/ssl_param.conf";
        include                 "/opt/bitnami/apps/blog.dongbumkim.com/conf/nginx-app.conf";
}
```

`default_server` 를 꼭 넣어줘야한다.

이 default_server 설정은 하나만 있어야하므로 여러 가상호스트를 쓰고 있다면 그 중 한 곳의 server 블록에만 넣어줘야한다. 만약 여러곳에 넣으면 nginx 실행시 에러난다.

SSL 설정은 다음 사이트에서도 만들 수 있다.

  * https://mozilla.github.io/server-side-tls/ssl-config-generator/
