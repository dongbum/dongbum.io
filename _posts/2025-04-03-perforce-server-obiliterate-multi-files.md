---
title: 퍼포스 서버에서 한번에 여러파일 정리하기
date: 2025-04-03T13:02:36+09:00
categories:
  - Perforce
tags:
  - perforce
  - vcs
---

퍼포스 서버를 관리하다보면 대량의 바이너리 파일들로 인해 서버 디스크의 용량이 점점 가득차게 된다. 적절하게 관리해주지 않으면 퍼포스 서버가 위태로워질 수 있으니 주기적으로 상태를 봐가며 관리해줘야한다. 특히 사용되지 않고 있는 오래된 리비전의 파일은 삭제하면 디스크 공간 확보에 많은 도움이 된다.

퍼포스에는 이 기능을 하도록 Obliterate Files 라는 기능이 있는데, 문제는 이 기능이 다 수동으로 작동되어야 한다는 것이다. 특정 리비전에서 특정 리비전까지 삭제는 가능하지만 이것을 각 파일마다 일일히 하나씩 다 처리해줘야한다.

처음에는 이것을 파일 하나씩 수동으로 정성스럽게 했었지만 관리해야할 파일이 너무 많아짐에 따라 손이 너무 많이 가서 자동으로 처리하도록 배치파일로 만들었다.

files_program.txt 파일에 정리할 파일들의 목록을 넣어놓으면 된다. 가장 최근 5개 리비전만 남기게 된다.

퍼포스 쓰는 사람들은 잘 알겠지만 Obliterate 명령은 한번 실행되면 절대 되돌릴 수 없으므로 아래 배치파일을 실행하기 전에 충분히 코드를 파악하고 테스트 파일에 테스트를 해보고 실행해보길 바란다.

```
@echo off
setlocal enabledelayedexpansion

rem 파일 목록이 저장된 파일 경로 설정
set FILE_LIST=files_program.txt

rem files.txt 파일을 한 줄씩 읽어서 처리한다.
for /f "delims=" %%a in (%FILE_LIST%) do (
	set "TARGET_FILE=%%a"
	
	rem echo !TARGET_FILE!
	
    rem 가장 마지막 리비전 번호를 구한다.
    for /f "tokens=3" %%b in ('p4 fstat -T headRev !TARGET_FILE! 2^>nul ^| find "headRev"') do set LATEST_REVISION=%%b

    rem echo !LATEST_REVISION!
	
	rem 마지막 5개 리비전이 5 초과일 때만 처리한다.
	if !LATEST_REVISION! gtr 5 (
        
	    set /a LATEST_REVISION -= 5
	    
	    rem echo !LATEST_REVISION!
	    
		rem obliterate 명령 실행
	    rem echo "p4 obliterate -T !TARGET_FILE!#1,#!LATEST_REVISION!"
	    
	    p4 obliterate -y -T !TARGET_FILE!#1,#!LATEST_REVISION!
    )
)

endlocal
```

files_program.txt 파일은 아래와 같이 작성한다.

```
//depot/Project/Server/TestServer1.pdb
//depot/Project/Server/TestServer2.pdb

//depot/Project/Server/TestServer3.pdb
//depot/Project/Server/TestServer4.pdb
```