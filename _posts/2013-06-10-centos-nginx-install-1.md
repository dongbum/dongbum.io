---
title: 'CentOS 에서 nginx 설치 #1'
date: 2013-06-10T16:58:13+09:00
categories:
  - Linux
tags:
  - CentOS
  - nginx
  - php-fpm
---
CentOS에서 nginx를 설치해본다.

nginx 사이트에서 레포지토리 파일을 받는다.

CentOS 6.4에서 yum으로 일단 편리하게 설치. php-fpm도 같이 설치한다.

php-fpm의 기본 설정 파일은 `/etc/php-fpm.d` 에 들어있다.

nginx 설정파일을 수정한다. php를 사용하기 위해 중요한 설정은 다음과 같다.

```nginx
server {
  listen 81;
  server_name www.dongbumkim.com; # 가상호스트가 쓸 도메인을 정의한다.

  #charset koi8-r;
  #access_log /var/log/nginx/log/host.access.log main;

  location / {
    # root /usr/share/nginx/html;
    root /home/sadasd/public_html/test/; # 루트 경로를 지정한다.
    index index.php index.html index.htm;
  }

  #location / {
  # proxy_pass_header Server;
  # proxy_set_header Host $http_host;
  # proxy_set_header X-Real-IP $remote_addr;
  # proxy_set_header X-Scheme $scheme;
  # proxy_pass http://127.0.0.1:80;
  #} # 이렇게 설정하면 프록시 설정하여 특정 도메인의 접속을 아파치로 넘길 수 있다.

  #error_page 404 /404.html;

  # redirect server error pages to the static page /50x.html
  #
  #error_page 500 502 503 504 /50x.html;
  #location = /50x.html {
  # root /usr/share/nginx/html;
  #}

  # proxy the PHP scripts to Apache listening on 127.0.0.1:80
  #
  #location ~ .php$ {
  # proxy_pass http://127.0.0.1;
  #}

  # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
  #
  location ~ .php$ {
    root /home/asdasd/public_html/test/;
    fastcgi_pass 127.0.0.1:9000;
    fastcgi_index index.php;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    include fastcgi_params;
  } # PHP를 사용하기 위한 설정 root 경로를 주의한다.

  # deny access to .htaccess files, if Apache's document root
  # concurs with nginx's one
  #
  location ~ /.ht {
  # deny all;
  #}
}
```

참고 URL : <http://amuzr.blog.me/90169647032>
