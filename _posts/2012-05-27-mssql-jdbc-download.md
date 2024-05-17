---
title: MSSQL JDBC 버전별 다운로드 방법
date: 2012-05-27T00:08:39+09:00
categories:
  - Java/JSP
tags:
  - Java/JSP
  - JDBC
  - MSSQL
  - MSSQL2012
  - 자바
---
요즘은 C와 윈도우프로그래밍을 하며 MSSQL을 좀 보고 있는 중이다.

어찌어찌하다보니 MSSQL의 데이터를 어떻게 웹에서 표현할까 생각해보다가 JSP에서 한번 보여줄 방법이 없나 찾아보았다.

PHP는 MSSQL 연결 모듈이 따로 있다. 내 서버의 CentOS에서는 PHP 패키지 버전이 5.3 버전이 되면서 레포지토리에서 제거되었다. (리눅스에서 MSSQL을 쓸일이 없을거라 생각해서 그런거였는지는 모르겠지만.)

어찌됐던 JSP에서 MSSQL에 접근하려면 JDBC 드라이버가 필요하다. ODBC나 아니면 다른 데이터베이스 연결방법이 있겠지만 역시나 제일 일반적인 방법이 JDBC일테고 나 역시도 JDBC에 익숙하니까. 어찌됐던 MSSQL용 JDBC 드라이버를 찾기 위해 검색해보았다.

...찾기 어렵다. 젠장.

네이버에서 찾아본 결과 예전 버전 JDBC만 자꾸 검색되었다.

마이크로소프트에서 찾아봤더니 너무 찾기 짜증났다. SQL 서버 홈페이지에서도 찾기 힘들고... 마이크로소프트 웹사이트는 검색하기도 힘들고 내가 원하는 검색결과가 잘 나오지도 않는다. 마이크로소프트 웹사이트는 검색기능을 좀 어떻게 바꿔줬으면 하는 생각이 참 크다.

나중에라도 다시 필요할까봐, 그리고 누군가가 필요할까봐 여기에 적어둔다.

  * MSSQL 서버용 JDBC 드라이버 4.0 : <http://www.microsoft.com/downloads/ko-kr/details.aspx?FamilyID=49C554CA-41A0-472C-B728-75DF5789369C>
  * MSSQL 서버용 JDBC 드라이버 3.0 : <http://www.microsoft.com/downloads/ko-kr/details.aspx?familyid=a737000d-68d0-4531-b65d-da0f2a735707>
  * MSSQL 서버용 JDBC 드라이버 2.0 : <http://www.microsoft.com/downloads/ko-kr/details.aspx?FamilyID=99B21B65-E98F-4A61-B811-19912601FDC9>

당연하겠지만 각 드라이버 버전별로 지원하는 MSSQL 서버 버전이 다르다. 해당하는 웹페이지에 가서 맞는 버전으로 받아야한다.

내 윈도우 서버의 경우에는 MSSQL 2012 버전을 사용 중이라 4.0 버전을 받아서 사용해야할 것 같다.

아직 사용해본 것은 아닌지라 사용방법이나 사용기는 나중에.
