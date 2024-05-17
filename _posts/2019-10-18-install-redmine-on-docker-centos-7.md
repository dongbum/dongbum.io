---
title: CentOS 7 에서 Docker로 Redmine 설치
date: 2019-10-18T14:46:13+09:00

categories:
  - AWS
  - GIT
  - Lightsail
  - Linux
tags:
  - AWS
  - CentOS
  - docker
  - linux
  - redmine
  - 도커
  - 레드마인
  - 리눅스
---
CentOS 7 에서 Docker로 Redmine을 설치해본다.

도커가 없이도 ruby부터 시작해서 rails, rake 등등 모든걸 하나하나 설치해나갈 수도 있지만 직접 해본 결과 이렇게 하면 온갖 오류에 시달리게 된다. 특히나 CentOS 7의 기본 ruby 버전이 2.0.0으로 많이 낮기 때문에 이 문제를 해결하는 것부터가 난관이다.

아무튼 도커로 편하게 사용하는 것을 추천한다.

---

### AWS Lightsail 서버에 설치

![](/assets/images/lightsail-choice-instance.png)

CentOS 7 선택 메모리 512MB로 실행한다.
인스턴스가 생성되면 고정IP를 부여하여 연결한다.

```console
yum check-update
```

명령으로 패키지 업데이트 할 것이 있는지 확인한 다음,

```console
sudo yum update -y
```

명령으로 모든 패키지를 업데이트하고 재부팅을 한다.

---

### MariaDB 설치

레드마인이 쓰는 DB는 MySQL이나 MariaDB나 아무 것이나 가능하다. 난 MariaDB로 설치.

CentOS 7에는 MariaDB가 이미 패키지내에 포함되어 있지만, 현재 기준으로 5.5.64 버전이 들어가 있다. 최신 버전으로 업데이트하기 위해 <https://downloads.mariadb.org> 으로 이동.

현재 기준으로 10.4.8 버전이 가장 최신의 stable 버전이다.

소스를 다운 받아서 컴파일 설치하기보다는 yum을 이용하는게 더 낫다.

<https://downloads.mariadb.org/mariadb/repositories> 로 이동하여 자기가 사용하는 배포판 버전을 선택하면 하단에 yum 설정이 출력된다. 그대로 복사하여 `/etc/yum.repos.d/MariaDB.repo` 경로로 파일을 만들고 내용에 붙여넣는다.

```console
sudo yum install MariaDB-server MariaDB-client
```

명령으로 MariaDB를 설치한다.

```console
sudo systemctl enable mariadb
```

명령으로 MariaDB를 서비스 시작으로 활성화한다.

```console
sudo systemctl start mariadb
```

명령으로 서비스 시작

```console
sudo systemctl status mariadb
```

입력하여 서비스 상태 확인한다.

명령으로 시스템 시작시 자동서비스 시작, MariaDB를 시작하고 제대로 설정되었는지 확인한다.

```console
sudo mysql_secure_installation
```

을 입력하여 DB 기초설정을 시작한다.

중간에 Switch to unix_socket authentication [Y/n] 이라고 나오며 입력을 요구하는데, 이에 대해서는 <http://www.nemonein.xyz/2019/07/2254/> 에 설명되어 있다. 시스템의 계정과 mysql 유저계정을 매칭시킬 것인지 여부인데 이걸 하게되면 보안에 좋을 것 같긴한데 기존의 사용방식에 헷갈릴 것 같아서 n 로 입력했다.

설정 과정에서 루트 암호를 변경해주고 테스트 테이블과 익명 접속을 차단해준다.

```console
mysql -u root -p
```

로 mariadb에 접속한다.

```sql
CREATE DATABASE redmine CHARACTER SET utf8 COLLATE utf8_general_ci; -- 데이터베이스 생성.
CREATE USER 'redmine'@'%' IDENTIFIED BY 'aa12341234'; -- 유저와 패스워드 생성
GRANT ALL PRIVILEGES ON redmine.* TO 'redmine'@'%'; -- 데이터베이스에 권한 부여
FLUSH PRIVILEGES; -- 입력한 내용 바로 적용
```

여기까지 진행하면 MariaDB 데이터베이스 설치가 끝난다.

---

### Redmine 설치

리눅스에서 레드마인을 설치하는 방법은,

  1. 일일히 다 하나씩 설치한다.
  2. 그냥 도커로 한방에 설치해버린다.

두가지가 있다.

1번을  선택하면 관련 라이브러리부터 시작해서 모두 다 하나씩 설치해야하는데 CentOS에서 이게 생각보다 쉽지 않다.

왜냐면 레드마인을 돌려야할 Ruby가 2.0.0 버전이 기본인데 이 버전으로는 gem이며 여러가지 프로그램에서 오류가 계속 발생한다. epel이나 scl 등의 추가 라이브러리를 쓰면 2.6 이상을 설치할 수 있지만 그것도 사실 쉽지는 않다.

개인적으로는 도커로 한방에 설치하는 것을 추천.

---

### Redmine 설치 : Docker로 레드마인 설치

<https://hub.docker.com/_/redmine> 에서 레드마인 공식 도커이미지를 받을 수 있다.

설정은 다음과 같은 파일을 만들어서 실행하면 된다.

```
docker run -d \
--name redmine \
-p 80:3000 \
-e DB_ADAPTER=mysql2 \
-e DB_HOST=localhost \
-e DB_PORT=3306 \
-e DB_USER=redmine \
-e DB_PASS=aa12341234 \
-e DB_NAME=redmine \
-v /home/centos/docker/redmine/datadir:/usr/src/redmine/files \
--restart="always" \
docker.io/redmine
```

모든 설정이 제대로 된줄 알았는데 접속이 되었다 안되었다 하였다. 이유를 찾아보니 시스템 메모리가 512MB 밖에 되지 않는데 이 메로리를 거의 다 사용하고 있었다.

다음과 같은 방법을 통해 스왑 메모리를 증가시켜 준다.

<https://dongbumkim.com/2016/12/10/add-swap-memory-in-linux>

이 방법을 쓰고 나면 오류가 없어졌다.

여기까지 했다면 일단 레드마인은 실행된다. 설정했던 포트 80번으로 접속하면 레드마인 접속이 가능하다. 관리자 초기비밀번호는 admin / admin

---

### 플러그인과 테마 설치

설치 이후 레드마인에 플러그인을 추가한다거나 테마를 추가하려면 도커 컨테이너 안에 들어가서 처리를 해야한다.

```console
docker exec -it redmine bash
```
명령으로 내부로 들어간 다음,

```console
apt-get update
```

를 한번 해주고 `/usr/src/redmine` 이 레드마인 루트 경로이다.

플러그인을 설치하겠다면 plugins 디렉토리에 가서 원하는 플러그인을 다운로드 받으면 된다.

테마는 public/themes 디렉토리에 원하는 테마를 다운로드한다.

테마는 다운로드하면 바로 적용해볼 수 있지만, 플러그인은 레드마인 컨테이너를 재시작해야만 적용된다.

```console
sudo docker ps
```

명령으로 컨테이너 ID를 확인한 후,

```console
sudo docker restart [컨테이너ID]
```

를 입력하여 컨테이너를 재시작해주면 플러그인이 추가된 것을 확인할 수 있다.

---

### 참고자료

  1. <https://brunch.co.kr/@cheuora/45>
  2. <https://jistol.github.io/its/2018/01/25/redmine-mysql-in-docker/>
  3. <http://www.kwangsiklee.com/2017/07/centos%EC%97%90%EC%84%9C-ruby%EB%A5%BC-rpm%EC%9C%BC%EB%A1%9C-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0/>
