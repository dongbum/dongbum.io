---
title: Lightsail 에서 NGINX에 SSL 적용하기 (2)
date: 2018-04-02T15:35:35+09:00

categories:
  - AWS
  - Lightsail
  - Linux
tags:
  - http
  - https
  - Lightsail
  - nginx
  - SSL
  - 라이트세일
  - 엔진엑스
  - 인증서
---
제대로 접속이 된다면 Lightsail 관리자페이지에서 HTTPS 접속을 위한 환경을 추가한다. ssl.dongbumkim.com 도메인을 추가한 다음, 443 포트를 연다.

![](/assets/images/ssl-open-port.png)

이제 SSL 설정을 시작한다.

내가 수정해야될 사이트의 가상호스트 설정 파일을 연다. 내 경우에는, `/opt/bitnami/apps/ssl.dongbumkim.com/conf/nginx-vhost.conf` 파일.

기존의 파일 내용은 다음과 같다.

```nginx
server {
        listen          80;
        root            /opt/bitnami/apps/ssl.dongbumkim.com/htdocs;
        server_name     ssl.dongbumkim.com;
        client_max_body_size    40M;
        include         "/opt/bitnami/apps/ssl.dongbumkim.com/conf/nginx-app.conf";
}
```

이 파일 내용을 다음과 같이 변경한다.

```nginx
server {
        listen          80;
        root            /opt/bitnami/apps/ssl.dongbumkim.com/htdocs;
        server_name     ssl.dongbumkim.com;
        client_max_body_size    40M;
        include         "/opt/bitnami/apps/ssl.dongbumkim.com/conf/nginx-app.conf";
}

server {
        listen                  443 ssl;
        root                    /opt/bitnami/apps/ssl.dongbumkim.com/htdocs;
        server_name             ssl.dongbumkim.com;
        ssl_certificate         /opt/bitnami/nginx/ssl/server.crt;
        ssl_certificate_key     /opt/bitnami/nginx/ssl/server.key;
        ssl_session_cache       shared:SSL:1m;
        ssl_session_timeout     5m;
        ssl_ciphers             HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers  on;
        include                 "/opt/bitnami/apps/ssl.dongbumkim.com/conf/nginx-app.conf";
}
```

수정이 완료되었다면 lightsail 재시작 명령어로 모두 재시작한다.

```
sudo /opt/bitnami/ctlscript.sh restart
```

재시작하고 난 뒤 웹브라우저를 열고 https://ssl.dongbumkim.com 으로 접속이 잘 되는지 확인해본다.

구글 크롬과 인터넷 익스플로러의 경우 다음과 같이 표시된다.

![](/assets/images/ssl-browser-warning.png)

내가 임의로 생성한 인증서이므로 안전하지 않기에 경고가 표시되지만 경고를 무시하면 어찌됐든 잘 표시된다.

만약, **HTTP로 접속하더래도 무조건 HTTPS로 강제로 접속시키고 싶다면**, `nginx-vhost.conf` 파일을 열고 다음과 같은 설정을 추가한다.

```nginx
server {
        listen          80;
        root            /opt/bitnami/apps/ssl.dongbumkim.com/htdocs;
        server_name     ssl.dongbumkim.com;
        client_max_body_size    40M;
        include         "/opt/bitnami/apps/ssl.dongbumkim.com/conf/nginx-app.conf";
        rewrite ^ https://$server_name:443?request_uri? permanent;
}

server {
        listen                  443 ssl;
        root                    /opt/bitnami/apps/ssl.dongbumkim.com/htdocs;
        server_name             ssl.dongbumkim.com;
        ssl_certificate         /opt/bitnami/nginx/ssl/server.crt;
        ssl_certificate_key     /opt/bitnami/nginx/ssl/server.key;
        ssl_session_cache       shared:SSL:1m;
        ssl_session_timeout     5m;
        ssl_ciphers             HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers  on;
        include                 "/opt/bitnami/apps/ssl.dongbumkim.com/conf/nginx-app.conf";
}
```

엔진엑스를 재실행하면 이제부터는 http로 접속하더라도 https 로 자동전환된다.

테스트하려면 http://ssl.dongbumkim.com 혹은 https://ssl.dongbumkim.com 으로 접속해보시길. (언제 닫을지는 모릅니다.)

참고자료

  * https://blog.naver.com/moonv11/221017860650
  * https://goo.gl/gBSf5b
  * http://dveamer.github.io/backend/SSLOnNginx.html
