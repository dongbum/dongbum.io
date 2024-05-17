---
title: 루아에서 간단한 파일 쓰기
date: 2013-04-08T18:39:42+09:00
categories:
  - Lua
---
루아를 연동하며 사용하다가 간단한 파일 쓰기가 필요해서 찾게된 코드.

정말 간단하지만 유용하게 사용했다.

```lua
-- 데이터 파일쓰기 시작
local f = assert(io.open("testtest.test", "w"))
f:write(simulation_data)
f:close()
-- 데이터 파일쓰기 종료
```
