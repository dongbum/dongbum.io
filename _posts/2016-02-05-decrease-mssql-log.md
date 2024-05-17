---
title: MSSQL 로그 파일 용량 줄이는 방법
date: 2016-02-05T11:12:01+09:00
categories:
  - Database/SQL
tags:
  - Database
  - MSSQL
  - SQL
  - 데이터베이스
  - 로그
---
개발용 MSSQL 서버를 사용하면서 로그 파일 용량이 점점 늘어갔다. 평소에는 별 상관 없는데 데이터베이스를 복사 떠가려고 하거나 백업할 때마다 엄청나게 많은 시간이 소요되니... 로그 파일이 무려 3기가가 넘었다. 그래서 로그 파일 용량을 줄였다. 데이터베이스 속성을 보면 다음과 같이 크기가 엄청나게 크다.

![](/assets/images/mssql-log-size.png)

다음의 코드를 입력한다.

![](/assets/images/mssql-descrease-log-size.png)

```sql
USE GameDB
SELECT DATABASEPROPERTYEX('GameDB', 'Recovery');
ALTER DATABASE GameDB SET RECOVERY SIMPLE
DBCC SHRINKFILE (GameDB_New_log, 10)
ALTER DATABASE GameDB SET RECOVERY FULL
```

쿼리 실행시 해당하는 데이터베이스에 가서 실행해야 하므로 USE를 한번 써준다. GameDB는 사용하는 데이터베이스 이름, GameDB_New_log는 로그 파일의 논리적 이름을 써주면 된다.

쿼리를 실행하면 몇초 걸리지 않고 굉장히 빨리 처리된다.

다시 데이터베이스 속성을 열어보면 다음과 같이 로그 파일 크기가 줄어있다.

![](/assets/images/mssql-descrease-log-size-success.png)
