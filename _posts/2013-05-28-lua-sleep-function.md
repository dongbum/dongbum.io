---
title: lua에서 sleep 기능 추가하기
date: 2013-05-28T10:42:18+09:00
categories:
  - Lua
tags:
  - Lua
  - sleep
  - 루아
---
lua에서 시간을 강제로 지연시킬 일이 있어 검색하다가 찾게된 내용이다.

C에서 sleep() 함수와 같이 사용해볼 수 있다.

```lua
local clock = os.clock
function sleep(n)  -- seconds
  local t0 = clock()
  while clock() - t0 <= n do end
end
```

출처 :: <http://lua-users.org/wiki/SleepFunction>
