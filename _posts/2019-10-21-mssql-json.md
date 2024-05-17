---
title: MSSQL에서 JSON 사용하기
date: 2019-10-21 16:48:38+09:00

categories:
  - Database/SQL
tags:
  - JSON
  - MSSQL
  - 데이터베이스
---
MSSQL에서 JSON을 지원하기 시작했다. 아래에서는 JSON을 다루기 위한 방법들이다.

  * <https://docs.microsoft.com/ko-kr/sql/relational-databases/json/json-data-sql-server>
  * <https://docs.microsoft.com/ko-kr/sql/relational-databases/json/validate-query-and-change-json-data-with-built-in-functions-sql-server?view=sql-server-ver15>

**중요사항 : MSSQL 2016 버전부터 JSON 기능을 지원한다.**

---

JSON 데이터를 저장하기 위한 컬럼 타입은 `NVARCHAR(MAX)` 를 사용한다.

INSERT 시 다음 처럼 입력한다.

```sql
INSERT INTO jsontest VALUES (1, N'{"EmployeeInfo": {
            "FirstName": "John",
            "LastName": "Doe",
            "Dob": "12-Jan-1970",
            "AnnualSalary": 85000
        }}')
```

SELECT 하면 JSON 데이터가 그대로 나온다.

DB의 한 컬럼에 그냥 JSON 데이터를 그대로 넣는다. 다만, MSSQL에 JSON 형식의 데이터를 다루기 위한 함수가 추가되어 있을 뿐이다.

---

### JSON 관련 함수

JSON 관련 중요 함수들.

---

#### ISJSON

JSON 데이터의 유효성을 판단한다. 이 함수를 통해 JSON 데이터가 맞는지 확인 가능하다. JSON 데이터가 맞다면 1을 리턴한다. `if (ISJSON(data))` 의 방법으로 데이터의 유효성을 검증한다.

---

#### JSON_VALUE

```sql
SELECT JSON_VALUE(json_data, '$.EmployeeInfo.FirstName') FROM jsontest
```

의 방법으로 json_data 안에 있는 특정 데이터를 가져올 수 있다.

참고자료에 있는 대로 같은 속성이 여러개 있다면 인덱스로 접근 가능하다. 인덱스는 0부터 시작한다.

하나 불편한 점은, 이렇게 SQL을 입력하는 과정에서 코드힌트 등의 기능은 작동하지 않는다. 내가 조회하려는 데이터의 속성명을 정확하게 입력해야만 한다.

---

### JSON_QUERY

지정된 속성 밑에 있는 문자열들을 JSON 데이터로써 리턴한다.

예를 들면,

```sql
SELECT JSON_VALUE(json_data, '$.EmployeeInfo') FROM jsontest
SELECT JSON_QUERY(json_data, '$.EmployeeInfo') FROM jsontest
```

EmployeeInfo 에 있는 데이터를 사용하고 싶을 때, 위의 것은 NULL이 반환되지만 밑에는 정상적으로 EmployeeInfo 밑의 데이터들이 출력된다.

---

### JSON_MODIFY

JSON 데이터의 값을 수정한다.

중요한 점은, SELECT 하면서 JSON_MODIFY를 사용하는 경우에는 결과 데이터에서 바뀔 뿐이지 테이블 내의 실제 데이터값이 변경되는 것은 아니다.

만약, 테이블내의 실제 데이터값을 바꾸려면 아래 코드처럼 사용한다.

```sql
SET @info = JSON_MODIFY(@jsonInfo, "$.info.address[0].town", 'London')
```

리턴되는 값은 새로 업데이트된 값을 반환한다.

만약 테이블에 있는 JSON 컬럼을 업데이트하고자 한다면 다음처럼 사용할 수 있다.

```sql
UPDATE [GameDB].[dbo].[ItemEquip]
SET [Value] = JSON_MODIFY([Value], '$.DyeList[1]', 0)
WHERE JSON_VALUE([Value], '$.DyeList[1]') = 4278190080
```

---

### OPENJSON

간단히 얘기해서, JSON 데이터를 RDBMS에서 흔히 사용하던 테이블 형식 즉, 컬럼/레코드 형식으로 바꾸어 반환한다.

테이블 형식으로 바뀐 데이터에 기존의 SQL처럼 SELECT 등을 할 수 있다.

다음처럼 사용 가능하다.

```sql
DECLARE @json NVARCHAR(MAX)
SET @json = (SELECT JSON_QUERY(json_data, '$.EmployeeInfo') FROM jsontest)

SELECT * FROM OPENJSON(@json)
```

이 함수는 SELECT 시에 바로 사용이 불가능하고 저렇게 변수에 일단 JSON 데이터를 받은 다음 거기에 다시 OPSNJSON 함수를 실행해야한다.

---

### FOR JSON

for json은 테이블에 있는 데이터들을 JSON 형식으로 내보내기 위한 함수이다.

```sql
SELECT * FROM jsontest FOR JSON AUTO
```

를 입력하면 테이블에 있는 데이터를 JSON 형식으로 변경하여 보여주게 된다.

---

### 참고자료

  * <https://aspdotnet.tistory.com/2149>
  * <https://riptutorial.com/ko/sql-server/example/17751/json-%EC%97%B4%EC%9D%98-%EA%B0%92-%EC%97%85%EB%8D%B0%EC%9D%B4%ED%8A%B8>
