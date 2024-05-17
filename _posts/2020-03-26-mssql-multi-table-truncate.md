---
title: MSSQL에서 여러 테이블 데이터 한번에 삭제하기
date: 2020-03-26T01:28:16+09:00
categories:
- Database/SQL
tags:
- MSSQL
- 데이터베이스
---
MSSQL을 사용 중인데 개발 중에는 DB를 초기화 해야할 경우가 종종 생긴다. 개발 DB이기 때문에 잘못된 데이터가 들어갈 때도 있고, 프로그램 코드가 변경됨에 따라 DB에 넣어야할 데이터의 구조가 달라지며 생기는 오류 등이 나오기 때문이다.

DB를 다 지워버리고 다시 생성하면 되지만 이렇게 하려면 테이블과 프로시져를 모두 다시 생성해야 한다. 내가 원하는 것은 테이블 내의 데이터만 삭제하는 것이었다.

DELETE 쿼리로 테이블의 내용을 하나하나 삭제하면 되지만 이것은 내가 하나하나의 테이블을 돌아다녀야 하는 것이며 개발하며 테이블 수가 늘어나면 늘어날수록 노가다를 더 해야한다는 것이다.

이를 자동화할 방법이 없을까 생각해보다가 다음과 같은 프로시져를 작성하였다.

```sql
USE [GameDB]
GO

SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER ON
GO

CREATE PROCEDURE [dbo].[GSP_GD_TABLE_TRUNCATE]
	@confirm			NVARCHAR(1024)
,	@o_result			INT		OUTPUT

AS

	SET NOCOUNT ON
	SET LOCK_TIMEOUT 30000
	SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED

	IF @confirm != N'truncate_confirm'
		RETURN

	SET @o_result = -1000;

	BEGIN TRAN
		DECLARE @TEMP_TABLE TABLE( seq int identity, table_name NVARCHAR(1024) )

		INSERT	@TEMP_TABLE
				SELECT	CONCAT('TRUNCATE TABLE ', TABLE_SCHEMA, '.', TABLE_NAME, ';')
				FROM	information_schema.tables
				WHERE	TABLE_CATALOG = 'GameDB'

		DECLARE @i int, @j int
		SELECT @i = 1, @j = @@ROWCOUNT

		WHILE @i <= @j
		BEGIN
			DECLARE @sql NVARCHAR(1024) = (SELECT table_name FROM @TEMP_TABLE WHERE seq = @i)
			EXEC( @sql )

			SET @i = @i + 1
		END

    -- 추가로 삭제하고 싶은 테이블들은 여기에 별도로 추가
		TRUNCATE TABLE [AccountDB].[dbo].[OtherTable]

	IF @@ERROR = 0
	BEGIN
		COMMIT TRAN
		SET @o_result = 0;
	END
	ELSE
	BEGIN
		ROLLBACK TRAN
		SET @o_result = @@ERROR
	END

GO
```

사용 방법은 프로시져를 실행할 때 인자에 truncate_confirm 를 입력하면 된다. 이것은 프로시져를 실수로 실행함을 방지하기 위함이다.

이 데이터베이스 외에도 다른 테이블을 더 삭제하고 싶다면 OtherTable 부분에 계속 추가해주면 된다.

프로시져를 실행하면 GameDB 데이터베이스 내부의 테이블 목록을 가져와서 해당 테이블들을 모두 truncate 해준다. truncate는 delete와 다르게 auto increment 값 등을 모두 초기화해준다.

개발DB 초기화 요청이 오면 프로시져 한번만 실행해주면 끝.
