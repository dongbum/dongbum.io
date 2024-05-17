---
title: MSSQL의 SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED
date: 2014-05-15T22:17:46+09:00
categories:
  - Database/SQL
tags:
  - Lock
  - SQL
---
MSSQL에서 프로시져 작성시 SELECT 하는 프로시져라면 다음의 문장을 넣는다.

```sql
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED
```

이 문장은 SELECT 할 때 WITH(NOLOCK) 옵션을 넣지 않아도 된다.
