---
title: Lightsail 에서 NGINX에 SSL 적용하기 (1)
date: 2018-04-02T15:34:56+09:00
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
Lightsail을 사용하며 SSL을 적용하는 방법에 대한 포스팅이다. SSL을 써본적도 없고 앞으로도 내가 이런걸 쓸일이 있을지는 모르겠으나 한번 시도해본다. (사실은 [푸우시로](http://blog.poohsiro.com)님의 요청으로)

나는 상용인증서를 구매할 생각이 없으므로 셀프로 생성해서 테스트해보기로 했다.

이제부터 쓸 내용은 AWS Lightsail NGINX 스택에서 설정한 것이며 운영체제는 우분투 리눅스, 웹서버는 nginx 이다. 굳이 lightsail에만 국한된 것은 아니고 어떤 리눅스라도 같은 과정을 거치면 HTTPS 설정이 가능하다. 상용인증서를 사용한다해도 인증서 공급업체의 인증서를 쓸뿐이지 다른 과정은 똑같다.

https://goo.gl/gBSf5b

을 참고하여 인증서를 생성한다.

**아래부터의 내용은 위 URL에 있는 개인인증서 생성을 직접 실행해본 것이다.**

```
bitnami@ip-172-26-10-176:~$ openssl version
OpenSSL 1.0.2l  25 May 2017
bitnami@ip-172-26-10-176:~$
```

명령으로 openssl이 설치되어 있는지 확인한다. 설치가 되어있지 않다면 `apt-get install openssl`로 openssl 패키지를 설치한다.

개인키를 생성한다.

```
bitnami@ip-172-26-10-176:~$ openssl genrsa -des3 -out server.key 2048
Generating RSA private key, 2048 bit long modulus
....................................+++
............................+++
unable to write 'random state'
e is 65537 (0x10001)
Enter pass phrase for server.key:
Verifying - Enter pass phrase for server.key:
bitnami@ip-172-26-10-176:~$
```

인증요청서를 생성한다. 몇가지 입력을 요구하는데 자기 상황에 맞도록 알아서 입력한다. 챌린지패스워드와 옵셔널 컴패니 네임은 입력하지 말라고 해서 그냥 엔터쳐서 넘기면 된다.

```
bitnami@ip-172-26-10-176:~$ openssl req -new -key server.key -out server.csr
Enter pass phrase for server.key:
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:KR
State or Province Name (full name) [Some-State]:Seoul
Locality Name (eg, city) []:Seadaemoon-gu
Organization Name (eg, company) [Internet Widgits Pty Ltd]:dongbumkim.com
Organizational Unit Name (eg, section) []:dongbumkim.com
Common Name (e.g. server FQDN or YOUR name) []:dongbumkim.com
Email Address []:YOUR_EMAIL@gmail.com

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
bitnami@ip-172-26-10-176:~$
```

개인키에서 패스워드 제거하기

```
bitnami@ip-172-26-10-176:~$ cp server.key server.key.origin
bitnami@ip-172-26-10-176:~$ openssl rsa -in server.key.origin -out server.key
Enter pass phrase for server.key.origin:
writing RSA key
bitnami@ip-172-26-10-176:~$
```

인증서 생성

```
bitnami@ip-172-26-10-176:~$ openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
Signature ok
subject=/C=KR/ST=Seoul/L=Seadaemoon-gu/O=dongbumkim.com/OU=dongbumkim.com/CN=dongbumkim.com/emailAddress=YOUR_EMAIL@gmail.com
Getting Private key
unable to write 'random state'
bitnami@ip-172-26-10-176:~$
```

여기까지 하고 나면, server.crt / server.csr / server.key / server.key.origin 이렇게 4개의 파일이 생긴다.

인증서 파일들을 넣을 폴더를 생성한다. 나는 `/opt/bitnami/nginx/ssl` 로 지정했다. 이걸로 할 필요는 없으니 자기 마음대로 결정한다. (예를 들면, 사이트마다 설정할 사람은 apps 디렉토리의 자기사이트 디렉토리 밑에 만드는게 더 나을 것이다.)

```
bitnami@ip-172-26-10-176:~$ sudo mkdir /opt/bitnami/nginx/ssl
```

생성된 3개의 파일을 인증서 관리용 디렉토리로 복사한다.

```
bitnami@ip-172-26-10-176:~$ sudo cp server.crt /opt/bitnami/nginx/ssl/
bitnami@ip-172-26-10-176:~$ sudo cp server.csr /opt/bitnami/nginx/ssl/
bitnami@ip-172-26-10-176:~$ sudo cp server.key /opt/bitnami/nginx/ssl/
```

SSL을 적용할 사이트에 설정을 시작한다.

난 SSL 테스트를 위한 것이므로 따로 SSL을 위한 사이트 설정을 하나 만들었다. 주소는 ssl.dongbumkim.com 으로 하기로 했다. 사이트 설정을 한다. vhost 등등....

일단 http://ssl.dongbumkim.com 으로 접속해본다. 만약 여기서 제대로 접속이 안된다면 nginx 가상호스트 설정이 제대로 안되었으니 제대로 나올 떄까지 수정해야된다.

글의 내용이 너무 길어져 두 포스팅으로 나눠서 써야겠다.
