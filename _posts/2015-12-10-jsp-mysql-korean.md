---
title: JSP에서 MySQL 데이터 입력시 한글 깨질 때 처리법
date: 2015-12-10T22:16:45+09:00
categories:
  - Database/SQL
---
로그 솔루션을 구축하던 중 JSP에서 MySQL에 데이터를  입력하다가 한글이 계속 깨졌다.

C++ 서버에서도 TCHAR로 한글을 처리하고 UTF-8로 맞추어 HTTP 전송했고, JSP 페이지들 모두 UTF-8 설정을 했고, JSP 파일 자체의 인코딩도 문제가 없었으며 `request.setCharacterEncoding("utf-8");` 코드도 입력된 상태였다.

이유를 몰라서 몇일을 헤메다가 해결방법을 찾았다.

MySQL 접속시 접속주소에 설정을 해주어야 한다.

나같은 경우 Commons 커넥션풀을 썼기 때문에 .jocl 파일 설정을 다음과 같이 변경했다.

```xml
<object class="org.apache.commons.dbcp.PoolableConnectionFactory" xmlns="http://apache.org/xml/xmlns/jakarta/commons/jocl">
  <object class="org.apache.commons.dbcp.DriverManagerConnectionFactory">
    <string value="jdbc:mysql://localhost:3306/log_conquest?useUnicode=true&characterEncoding=utf-8" />
    <string value="root" />
    <string value="root_passwd" />
  </object>
  <object class="org.apache.commons.pool.impl.GenericObjectPool">
    <object class="org.apache.commons.pool.PoolableObjectFactory" null="true" />
  </object>
  <object class="org.apache.commons.pool.KeyedObjectPoolFactory" null="true" />
  <string null="true" />
  <boolean value="false" />
  <boolean value="true" />
</object>
```

중요한 부분은 MySQL 접속 주소를 쓸 때 뒤에 붙는 파라미터이다.

**useUnicode=true&characterEncoding=utf-8** 을 붙여야만 정상적으로 UTF-8로 된 한글이 입력되게 된다.
