---
title: MySQL에서 현재 시간 자동 입력 방법
date: 2014-04-11T16:45:23+09:00
categories:
  - Database/SQL
tags:
  - MySQL
---
MySQL에서 현재 시간을 구하고 자동으로 입력하려면 다음의 쿼리를 이용한다.

```sql
INSERT INTO time_test VALUES ((SELECT NOW()));
```

로그 정보를 DB에 입력할 때 유용하게 사용했다. 아예 컬럼 기본값으로 함수를 지정하는 방법이 있지 않을까 싶은데 나중에 한번 찾아봐야할듯.

출처 : <http://blog.naver.com/yoonhok_524?Redirect=Log&logNo=60170357042>
