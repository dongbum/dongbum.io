---
title: 디버그 정보가 없는 것처럼 개체를 링크합니다. 에러 수정
date: 2017-12-26T18:12:30+09:00
categories:
  - C/C++
---
비주얼스튜디오에서 라이브러리 프로젝트로 프로젝트 컴파일시 다음과 같은 에러가 발생하는 경우,

```
1>------ 모두 다시 빌드 시작: 프로젝트: GameServer, 구성: Debug x64 ------
1>  game_protocol.cpp
1>  main.cpp
1>  game_server.cpp
1>  코드를 생성하고 있습니다...
1>     D:\Work\DServer2.git\vs_solution\Lib\GameServer64D.lib 라이브러리 및 D:\Work\DServer2.git\vs_solution\Lib\GameServer64D.exp 개체를 생성하고 있습니다.
1>DServer64D.lib(basic_socket.obj) : warning LNK4099: 'DServer.pdb' PDB를 'DServer64D.lib(basic_socket.obj)' 또는 'D:\Work\DServer2.git\vs_solution\Lib\DServer.pdb'에서 찾을 수 없습니다. 디버그 정보가 없는 것처럼 개체를 링크합니다.
1>DServer64D.lib(config.obj) : warning LNK4099: 'DServer.pdb' PDB를 'DServer64D.lib(config.obj)' 또는 'D:\Work\DServer2.git\vs_solution\Lib\DServer.pdb'에서 찾을 수 없습니다. 디버그 정보가 없는 것처럼 개체를 링크합니다.
1>DServer64D.lib(log_manager.obj) : warning LNK4099: 'DServer.pdb' PDB를 'DServer64D.lib(log_manager.obj)' 또는 'D:\Work\DServer2.git\vs_solution\Lib\DServer.pdb'에서 찾을 수 없습니다. 디버그 정보가 없는 것처럼 개체를 링크합니다.
1>DServer64D.lib(base_protocol.obj) : warning LNK4099: 'DServer.pdb' PDB를 'DServer64D.lib(base_protocol.obj)' 또는 'D:\Work\DServer2.git\vs_solution\Lib\DServer.pdb'에서 찾을 수 없습니다. 디버그 정보가 없는 것처럼 개체를 링크합니다.
1>  GameServer.vcxproj -> D:\Work\DServer2.git\vs_solution\Lib\GameServer64D.exe
1>  GameServer.vcxproj -> D:\Work\DServer2.git\vs_solution\Lib\GameServer64D.pdb (Full PDB)
========== 모두 다시 빌드: 성공 1, 실패 0, 생략 0 ==========
```

사실 프로그램 작동에는 문제가 없지만 나중에 디버깅시에 문제가 된다.

<http://shaeod.tistory.com/411> 를 참조해서 해결 완료.

문제 해결 방법은

  1. "구성 속성" -> "C/C++" -> "일반" -> "디버그 정보 형식" : "프로그램 데이터베이스(/Zi)" 로 수정
  2. "구성 속성" -> "C/C++" -> "코드 생성" -> "최소 다시 빌드 가능" : "아니요(/Gm-)"로 수정
  3. "구성 속성" -> "C/C++" -> "명령줄" -> "추가 옵션"에 "/Ylsymbol"를 입력

하면 해결된다.
