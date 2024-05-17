---
title: CentOS 6.2에서 phpMyAdmin mcrypt 오류 대처 방법
date: 2012-02-23T17:45:10+09:00
categories:
  - Linux
tags:
  - CentOS
  - libmcrypt
  - linux
  - mcrypt
  - php-mcrypt
  - phpMyAdmin
---
phpMyAdmin을 잘 사용하고 있는데 밑에 mcrypt 모듈이 없다며 계속 경고가 뜬다.

사실 그냥 내버려둬도 사용하는데는 별 지장 없지만 모든 창마다 계속 빨강색으로 뜨는게 별로 보기 좋지 않아서 mcrypt 모듈을 설치하기로 함.

CentOS 6.2에는 php-mcrypt가 같이 설치되지 않는다. CentOS 레포지토리에 이 모듈이 없기 때문이다. CentOS 5.7에는 이게 있었는데 PHP버전이 5.3으로 올라가면서 어찌된 일인지 이 모듈이 제외되었다. 하여튼 이걸 설치하려면 기본 레포지토리로는 안되는데... 처음에는 libmcrypt 모듈을 다운받아서 컴파일 한다음 설치하려했다. 설치는 되긴했는데 PHP와 연결모듈을 또 찾아서 설치해야되는 귀찮음이 있었다.

구글을 뒤져보니 EPEL 즉, 레드햇 엔터프라이즈 리눅스 추가 패키지에 php-mcrypt 모듈이 있다는 것을 발견하고 설치 시작. 인터넷의 글들에 있는 `download.fedora.redhat.com` 서버에는 링크가 깨져있다. 에효... URL이 잘못된건지 아니면 레드햇에서 패키지를 제거한건지.

하여튼 설치해야할 파일명은 `epel-release-6-5.noarch.rpm` 임을 알게되었으니 가장 찾기 편한 rpmfind.net 에서 검색해봤다.

당연히 있다. 🙂

<http://fr2.rpmfind.net/linux/rpm2html/search.php?query=epel-release&submit=Search+...>

에 가보니 여러가지 패키지별로 준비되어있다. 어차피 시스템 아키텍쳐와 상관없는 noarch 패키지이기 때문에 대충 맞는 것으로 다운로드. 내 경우에는 CentOS 6.2 64비트 버전이므로 <ftp://fr2.rpmfind.net/linux/epel/6/ppc64/epel-release-6-5.noarch.rpm> 를 다운받기로 했다. 적당한 디렉토리에 wget 명령어로 다운 받은 다음, rpm -ivh 명령으로 설치했다.

저 패키지를 설치한 다음 다시 `yum list php-mcrypt` 를 해보면 바로 잡힌다. `yum install php-mcrypt` 명령을 내리면 의존성으로 libmcrypt 패키지도 같이 설치된다. 설치가 잘되면 웹서버를 한번 재시작한 다음 phpMyAdmin을 켜보면 빨강색 경고 메시지가 사라지게 된다. 🙂

epel 패키지를 설치하고 `yum check-update` 를 해보면 몇가지 패키지 업데이트가 잡힌다. 그냥 그대로 업데이트. 🙂
