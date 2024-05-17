---
title: CentOS에 FreeTDS 설치하기
date: 2013-12-05T16:06:49+09:00
categories:
  - C/C++
  - Database/SQL
  - Linux
tags:
  - FreeTDS
  - linux
  - MSSQL
  - ODBC
  - 리눅스
---
### Step 1. 소스 다운로드

<http://www.freetds.org> 에서 FreeTDS 소스를 받는다.

### Step 2. 압축 해제

다음의 명령으로 압축을 푼다.

```
gunzip freetds-stable.tgz
```

압축을 풀면 tar 파일이 하나 나온다. tar xvf 명령으로 압축을 해제한다.

```
tar xvf freetds-stable.tar
```

### Step 3. 컴파일

압축 해제가 되면 freetds 폴더가 나오는데 여기에 들어가서 컴파일을 시작한다. configure / make / make install 명령으로 실행한다. 난 소스 설치할 때 위치를 지정해서 설치하는 편이라 --prefix 로 위치를 지정했다.

```
[dongbum@localhost freetds-0.91]$ ./configure --prefix=/usr/local/FreeTDS --enable-msdblib
[dongbum@localhost freetds-0.91]$ make
[dongbum@localhost freetds-0.91]$ make install
```

실행을 다 하고 나면 /usr/local/FreeTDS 에 freetds 관련 파일들이 설치된다.

### Step 4. 라이브러리 파일 경로 설정

`/etc/profile` 에 `LD_LIBRARY_PATH`에 freetds의 라이브러리 파일들을 연결해준다. /etc/profile 파일을 vi로 연다음 다음처럼 추가한다.

```
LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/FreeTDS/lib
```

설정이 적용되도록 재부팅을 한번 해준다.

### Step 5. odbc.ini 파일 수정

`/etc/odbc.ini` 파일에 내가 사용할 DB의 이름을 설정해준다. 내 경우에는 freetds 외에도 sqlite 설정도 같이 추가하였다.

```ini
[111.222.333.444]
Driver = FreeTDS
Description = 111.222.333.444
Trace = No
Servername = 111.222.333.444
Database = 기본DB이름

[TEST_SQLITE3]
Description = TEST SQLITE
Driver = SQLite
Database = /test/sqlite/test_sqlite.db
StepAPI = NO
```

### Step 6. odbcinst.ini 파일 수정

/etc/odbcinst.ini 파일에도 다음과 같은 설정을 추가해준다. 역시 SQLite를 사용하기 위한 설정도 추가하였다.

```ini
[FreeTDS]
Description = v0.64 with protocol v4.2
Driver = /usr/lib64/libtdsodbc.so
UsageCount = 5

[SQLite]
Description = ODBC for SQLite
Driver = /usr/local/lib/libsqlite3odbc.so
Setup = /usr/local/lib/libsqlite3odbc.so
FileUsage = 1
CPTimeout =
CPReuse =
```

/usr/lib64/libtdsodbc.so 파일은 /usr/local/FreeTDS/lib/libtdsodbc.so 파일을 향하도록 심볼릭링크를 걸어준다.

### Step 7. freetds.conf 파일 수정

마지막으로 freetds.conf 파일을 수정해준다. 난 /usr/local/FreeTDS에 설치하였으므로 /usr/local/FreeTDS/etc/freetds.conf 파일을 수정해주면 된다.

global 섹션에

```ini
client charset = EUCKR
text size = 4290000000
tds version = 4.2
```

를 추가한다. 캐릭터셋과 tds 버전 등을 지정하는 문구 같은데 이 옵션에 대해 정확히는 나도 잘 모르겠다. 그리고 아까 추가한 서버이름을 그대로 추가해준다.

```ini
[111.222.333.444]
host = 111.222.333.444
port = 1433
tds version = 8.0
```

여기까지 다 설정하고나면 FreeTDS 사용이 가능하다.

리눅스에서 MSSQL 접속이 쉬운게 아니란걸 다시 한번 느낀다.
