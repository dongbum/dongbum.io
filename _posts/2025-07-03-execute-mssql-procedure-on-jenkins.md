---
title: MSSQL 프로시저 실행 중 데드락. 원인과 해결까지
date: 2025-07-03T08:21:00+09:00
categories:
  - Database/SQL
  - DevStory
tags:
  - database
  - db
  - sql
  - mssql
  - jenkins
  - gameserver
---

##### 데드락! 무슨 일이 있었나

게임 패치를 위해 퍼블리셔에게 DB 패치 스크립트를 한데 묶어 전달했다.
서버 업데이트 당일, 모든 스크립트는 정상적으로 처리됐지만… 단 하나의 스크립트에서 모든 샤드에서 에러가 발생했다.

퍼블리셔 Tech PM이 전달해준 **5천 줄짜리 로그**를 샅샅이 뒤져보던 중, 눈에 띄는 메시지를 발견했다.

```
# [tid:19][ERROR] (1205, b'Transaction (Process ID 155) was deadlocked on lock | communication buffer resources with another process and has been chosen as the deadlock victim. Rerun the transaction.DB-Lib error message 20018, severity 13:\nGeneral SQL Server error: Check messages from the SQL Server\n') 201703
Exception in thread Thread-13:
Traceback (most recent call last):
  File "src/pymssql.pyx", line 448, in pymssql.Cursor.execute
  File "src/_mssql.pyx", line 1064, in _mssql.MSSQLConnection.execute_query
  File "src/_mssql.pyx", line 1096, in _mssql.MSSQLConnection.execute_query
  File "src/_mssql.pyx", line 1294, in _mssql.MSSQLConnection.get_result
  File "src/_mssql.pyx", line 1639, in _mssql.check_cancel_and_raise
  File "src/_mssql.pyx", line 1683, in _mssql.maybe_raise_MSSQLDatabaseException
_mssql.MSSQLDatabaseException: (1205, b'Transaction (Process ID 148) was deadlocked on lock | communication buffer resources with another process and has been chosen as the deadlock victim. Rerun the transaction.DB-Lib error message 20018, severity 13:\nGeneral SQL Server error: Check messages from the SQL Server\n')

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/home/infra/py/woodstock/execute_batch_sql/execute_batch_all_sql_use_thread_mssql.py", line 209, in process_restore
    p_db.cursor.execute(query)
  File "src/pymssql.pyx", line 468, in pymssql.Cursor.execute
pymssql.OperationalError: (1205, b'Transaction (Process ID 148) was deadlocked on lock | communication buffer resources with another process and has been chosen as the deadlock victim. Rerun the transaction.DB-Lib error message 20018, severity 13:\nGeneral SQL Server error: Check messages from the SQL Server\n')
```

클래식한 **데드락 에러**였다. 조금 더 자세히 보면 다음과 같은 `pymssql` 예외 트레이스백도 확인할 수 있었다.

#### 테스트할 땐 멀쩡했는데?

문제의 스크립트는 내부 QA 테스트에서는 아무런 문제가 없었다. 직접 SSMS에서 한 번에 실행했을 때는 에러도 없고, 처리 시간도 양호했다.

하지만 퍼블리셔 환경은 달랐다.  
그들은 **젠킨스에서 파이썬 스크립트 (`pymssql`)로**, **멀티스레드**로 동시에 각 샤드에 SQL을 실행하고 있었던 것 같다. (정확히는 확인할 수 없지만 정황상 꽤 유력하다.)

알고 보니 내가 작성한 쿼리는 다음과 같은 **데드락 발생 조건**을 잔뜩 갖추고 있었다:

- `UPDATE ... JOIN` 구문  
- 여러 테이블을 동시에 수정  
- UDF (사용자 정의 함수) 호출  

단일 세션에서는 잘 돌아가지만, 멀티세션/멀티스레드 상황에서는 충돌이 일어날 수밖에 없었다.

##### 해결책은 의외로 간단했다

쿼리를 리팩터링해서 테이블 접근 순서를 맞추거나, 복잡한 힌트를 주는 방법도 고려했지만...

가장 간단하고 확실한 해결책은 **단독 실행**이었다.

퍼블리셔 측에 "이 쿼리만은 병렬 실행하지 말고 단독으로 실행해 달라"고 요청했다. 그리고 실제로 그렇게 하자 문제는 바로 해결되었다.

#### 마무리

데드락은 언제나 예상치 못한 상황에서 등장한다. 특히 **멀티스레드 + 복잡한 쿼리** 조합은 사고가 나기 쉬운 구조다.

테스트할 땐 멀쩡했는데 실서버에서만 문제가 생긴다면, "이 쿼리가 병렬 실행될 가능성은 없었을까?" 한 번쯤 되짚어보는 것도 좋은 습관이다.