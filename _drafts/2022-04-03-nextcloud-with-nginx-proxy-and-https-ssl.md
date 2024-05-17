nginx 에 https 연동

root@debian:/disk1/docker/nginx-proxy/conf.sites# docker ps -a 로 확인


CONTAINER ID   IMAGE                           COMMAND                  CREATED        STATUS                PORTS                                                                                                                                                                                                                                                                                                                                                 NAMES
d79c1698379c   nextcloud                       "/entrypoint.sh apac…"   15 hours ago   Up 14 minutes         80/tcp, 443/tcp                                                                                            


root@debian:/disk1/docker/nginx-proxy/conf.sites# docker exec -it d79c1698379c /bin/bash

로 접속



/etc/apache2/sites-available/default-ssl.conf 파일을 연다.

SSLCertificateFile 과 SSLCertificateKeyFile을 방금 발급받은(마운트된) 경로를 입력해줍니다.

여기서 주의해야 할 점은,

SSLCertificateFile 에는 fullchain,pem 파일을,
SSLCertificateKeyFile 에는 privkey.pem 파일을
넣어줘야 합니다.

저장하고 닫는다.




/var/www/nextcloud/config 로 이동


vi config.php

로 파일을 연다.


'overwriteprotocol' => 'https', 추가한다.

'overwrite.cli.url' => 'https://cloud.example.com/', 에서 자기 도메인에 https 를 붙인다.

저장한다.


나와서 docker-compose restart [nextcloud 컨테이너명] 을 입력한다.


참고사이트 : https://zerogyun.dev/2020/01/29/NextCloud%EC%97%90%20LetsEncrypt/
참고사이트 : https://breuer.dev/tutorial/Setup-NextCloud-FrontEnd-Nginx-SSL-Backend-Apache2
