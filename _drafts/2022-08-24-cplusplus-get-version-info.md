C++에서 현재 실행 중인 실행파일의 버전 정보를 가져온다.

RzTChar szFilePath[ MAX_FILE_PATH_LENGTH ] = { 0, };
::GetModuleFileName( NULL, szFilePath, MAX_FILE_PATH_LENGTH );

RzTChar szBuffer[ MAX_PATH ] = { 0, };
if ( false != ::GetFileVersionInfo( szFilePath, 0, MAX_PATH, szBuffer ) )
{
    VS_FIXEDFILEINFO* pFineInfo = NULL;
    UINT bufLen = 0;
    // VS_FIXEDFILEINFO 정보 가져오기
    if ( FALSE != VerQueryValue( szBuffer, _T( "\\" ), (LPVOID*)&pFineInfo, &bufLen ) != 0 )
    {
        WORD majorVer, minorVer, buildNum, revisionNum;
        majorVer = HIWORD( pFineInfo->dwFileVersionMS );
        minorVer = LOWORD( pFineInfo->dwFileVersionMS );
        buildNum = HIWORD( pFineInfo->dwFileVersionLS );
        revisionNum = LOWORD( pFineInfo->dwFileVersionLS );

        // 파일버전 출력
        printf( "version : %d,%d,%d,%d\n", majorVer, minorVer, buildNum, revisionNum );
    }
}