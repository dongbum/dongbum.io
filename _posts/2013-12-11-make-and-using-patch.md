---
title: patch 파일 만들고 적용하기
date: 2013-12-11T09:35:18+09:00
categories:
  - Linux
  - Programming
tags:
  - linux
  - patch
  - 리눅스
---
### patch 파일 만들기

```
diff -u A.txt B.txt > change.patch
```

반드시 -u 를 붙여줘야 제대로 만들어진다.

### patch 파일 적용하기

```
patch -p0 < change.patch
```
