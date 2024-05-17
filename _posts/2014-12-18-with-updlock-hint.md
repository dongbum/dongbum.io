---
title: WITH(UPDLOCK) 힌트와 MSSQL에서 락 해제하는 방법
date: 2014-12-18T15:10:20+09:00
categories:
  - Database/SQL
tags:
  - Database
  - DB
  - Lock
  - 데이터베이스
  - 락
---
MSSQL에서 의도치 않게 락이 걸리는 상황이 발생했다. 내가 만든게 아니니 뭐...

에러가 나는 프로시져를 찾아보니 UPDATE 쿼리에서 `WITH(UPDLOCK)` 구분이 있었다.

쭈기님의 블로그에 있는 글 (<http://jjugi0606.tistory.com/30>)에 의하면 이 힌트를 쓰는 경우, WHERE 검색에 들어가는 컬럼들은 인덱스 설정이 되어야만 한다고 한다. 그래서 인덱스를 일단 설정. 앞으로 또 락이 걸리는지는 두고봐야 알 수 있을 것 같다.

어찌됐던 MSSQL에서 걸려있는 락을 해제하기 위한 방법도 찾아보았다. 이 글 (<http://yongandju.tistory.com/74>)에서 방법을 찾았다.

```sql
EXEC sp_lock
```

명령어를 통해 락이 걸린 부분을 찾는다. (Mode 부분이 X 표시가 된 것이 락이 걸린 것이다.)

```sql
dbcc inputbuffer(해당 spid)
```

를 입력해서 어떤 쿼리에서 락이 걸렸는지 확인한다.

```sql
kill 해당spid
```

를 입력해서 강제종료시킨다.

어찌됐던 위 과정을 통해서 락은 해제되었다.
