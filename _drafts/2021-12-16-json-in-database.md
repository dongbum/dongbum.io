---
title: RDBMS에서 JSON 사용하기
date: 2021-12-16T23:54:36+09:00
categories:
  - Database
tags:
  - Database
  - JSON
  - MSSQL
---

개발을 해오면서 데이터베이스를 사용할 때는 누구나 그러하듯 컬럼을 만들고 한 row 마다 데이터를 하나씩 저정하는 RDBMS의 전통적인 방식을 그대로 사용해오고 있었습니다. 대부분은 프로시져를 사용해서 DB를 다루고 있었구요. 가끔씩 생쿼리가 필요할 때도 있었겠죠.

새로운 회사에 입사하며 전혀 새로운 방식을 보게되었습니다.

RDBMS에 데이터를 JSON 형태로 저장하는 것입니다.

회사에서는 MSSQL을 사용하고 있었고 여기에 저장할 때 컬럼타입을 NVARCHAR로 하여 그 안에 JSON 포맷의 문자열을 저장하는 것입니다.

게임서버가 이 값을 DB로부터 읽어올 떄는 구조체를 이용해 Deserialization하여 사용하고, 반대로 DB에 써야할 때는 Serialization하여 쓰게 합니다.

이 기능을 해주는 유틸리티 프로그램도 물론 만들어져 있습니다.

이렇게하여 새로운 DB를 쓰게 되었습니다. 마치 몽고DB 같은 NoSQL을 쓰는 것처럼 RDB를 사용하게 된 것입니다.


#### 그렇게 했을 때 장점.

게임서버에서 사용하는 구조체의 변경에서 매우 자유롭습니다. DBA에게 요청하지 않아도 되고 개발자가 직접 DB 테이블의 구조를 수정하지 않아도 저장구조를 쉽게 변경할 수 있습니다.



#### 그렇게 하고 나니 단점.

저장된 데이터를 유지한채로 구조를 바꾸는 것이 어렵습니다.

만약에 이미 라이브가 시작되고 난 후라면, 이미 저장된 저장된 JSON 구조를 변경한다는게 쉽지 않습니다. RDB라면 저장된 데이터를 그대로 두고 컬럼명을 바꾸거나 컬럼타입을 바꾼다거나 하는게 비교적 어려운 일이 아니지만, JSON 타입의 포맷으로 저장되어 버리니 만약에 변수명(RDB의 컬럼명)이 바뀐다면 기존의 데이터들은 모두 날라가는 셈이 됩니다. 기존의 데이터를 그대로 유지하기 위해서는 DB 차원에서 데이터를 마이그레이션 해주는 작업을 해주어야 하는데 데이터의 구조가 JSON 이다보니 이것을 변경해주는 쿼리를 만드는 것조차도 쉬운 일은 아닙니다.

더군다나, 특정한 값의 이름이나 타입이 하나만 바뀌면 전체 모든 데이터들을 다 수정해줘야한다는 문제점이 있습니다.
