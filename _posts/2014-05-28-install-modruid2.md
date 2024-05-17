---
title: mod_ruid2 설치 방법
date: 2014-05-28T21:08:58+09:00
categories:
  - Linux
tags:
  - Apache
  - linux
  - mod_ruid2
  - 아파치
---
Apache 2 버전에서 mod_ruid2 를 설치하기로 했다.

<https://github.com/mind04/mod-ruid2> 에 가서 최신버전의 zip 파일을 다운로드한다. 다운로드한 파일을 서버에 올리고 unzip master.zip 명령을 내려 압축을 해제한다. 이 글을 쓰고 있는 시점에 최신 버전은 0.9.8 버전이다.

참고로, 내 서버의 환경은 CentOS 6.5 이며 모든 최신업데이트가 전부 적용되어 있다. Apache는 CentOS에서 제공하는 아파치를 사용하고 있다.

```console
apxs -a -i -l cap -c mod_ruid2.c
```

명령을 내리면 컴파일이 시작된다.

```console
----------------------------------------------------------------------
 Libraries have been installed in:
 /usr/lib64/httpd/modules
 If you ever happen to want to link against installed libraries
 in a given directory, LIBDIR, you must either use libtool, and
 specify the full pathname of the library, or use the `-LLIBDIR'
 flag during linking and do at least one of the following:
 - add LIBDIR to the `LD_LIBRARY_PATH' environment variable
 during execution
 - add LIBDIR to the `LD_RUN_PATH' environment variable
 during linking
 - use the `-Wl,-rpath -Wl,LIBDIR' linker flag
 - have your system administrator add LIBDIR to `/etc/ld.so.conf'
 See any operating system documentation about shared libraries for
 more information, such as the ld(1) and ld.so(8) manual pages.
 ----------------------------------------------------------------------
 chmod 755 /usr/lib64/httpd/modules/mod_ruid2.so
 [activating module `ruid2' in /etc/httpd/conf/httpd.conf]
 [root@83rpm mod_ruid2-0.9.8]#
 ```

메시지가 나오고 컴파일이 종료된다.

혹시 sys/capability.h 파일이 없다는 메시지가 나온다면 lilbcap-dev 패키지를 설치하면 해결된다.

아파치 폴더의 httpd.conf 를 보면,

```
LoadModule ruid2_module       /usr/lib64/httpd/modules/mod_ruid2.so
```

처럼 모듈 로딩 명령이 들어가 있다.

가상호스트 설정에 들어가서 다음의 내용을 추가한다.

```apache
<IfModule mod_ruid2.c>
 RMode config
 RUidGid 원하는유저ID 원하는그룹ID
</IfModule>
```

다 입력했다면 아파치 서버를 재시작하고 다음의 내용의 PHP 파일을 만들어서 제대로 작동하는지 확인한다.

```php
<?php
 makedir("testdir");
?>
```

#### 참고자료
  이 내용은 <http://fullpowe.blog.me/10158911404>를 참조했다.

**추가**

위 php 파일의 makedir() 함수로 테스트를 하려했으나 mod_security 모듈에 의한 보안정책 위반으로 제대로 실행되지 않았다. 자체적으로 테스트해본 결과, mod_ruid2 모듈은 정상적으로 작동하는 것을 확인하였다.
