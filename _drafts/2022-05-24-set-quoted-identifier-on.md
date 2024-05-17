다음과 같은 에러메시지에서

[22-05-23 20:37:41.917.495][ERROR][RzUser::CreateCharacter(575)] RZ_CHECK_RESULT( eResultCode ) : FailedToExcuteQuery[-180]
[22-05-23 20:37:49.498.952][ERROR][RzOdbcError::operator ()(22)] Failed to Execute Query, SQLExecute[STATE[42000], ERROR[1934], MSG[[Microsoft][ODBC SQL Server Driver][SQL Server]DELETE failed because the following SET options have incorrect settings: 'QUOTED_IDENTIFIER'. Verify that SET options are correct for use with indexed views and/or indexes on computed columns and/or filtered indexes and/or query notifications and/or XML data type methods and/or spatial index operations.]], Query[GSP_GD_Prolog_Data_D ?,?,?]
[22-05-23 20:37:49.499.052][ERROR][RzOdbcError::operator ()(29)] Failed to Execute Query, SQLExecute[STATE[42000], ERROR[1934], MSG[[Microsoft][ODBC SQL Server Driver][SQL Server]DELETE failed because the following SET options have incorrect settings: 'QUOTED_IDENTIFIER'. Verify that SET options are correct for use with indexed views and/or indexes on computed columns and/or filtered indexes and/or query notifications and/or XML data type methods and/or spatial index operations.]], Query[GSP_GD_Prolog_Data_D ?,?,?]
[22-05-23 20:37:49.499.101][ERROR][RzUser::deletePrologueCharacter(2116)] Failed to excute GSP_GD_Prolog_Data_D. (CharacId = 6934461786482516992)
[22-05-23 20:37:49.499.125][ERROR][RzUser::CreateCharacter(575)] RZ_CHECK_RESULT( eResultCode ) : FailedToExcuteQuery[-180]


SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO

을 추가해준다.



참고자료

https://stackoverflow.com/questions/1243991/update-failed-because-the-following-set-options-have-incorrect-settings-quoted
https://docs.microsoft.com/ko-kr/sql/t-sql/statements/set-quoted-identifier-transact-sql?view=sql-server-ver16