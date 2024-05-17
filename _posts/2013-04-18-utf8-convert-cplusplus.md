---
title: C++에서 문자열 UTF-8 변경 방법
date: 2013-04-18T10:54:23+09:00
categories:
  - C/C++
tags:
  - C
  - UTF-8
---

euc_kr의 문자열을 UTF-8로 바꿀 때 다음의 코드를 사용한다.

```cpp
std::string utf8_str = boost::locale::conv::to_utf< char >( team_name.c_str(), "EUC-KR" );
```

boost의 기능이 참 많다. 언제 시간내서 쭉 읽어봐야할텐데.
