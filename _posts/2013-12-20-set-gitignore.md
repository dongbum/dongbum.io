---
title: .gitignore 파일 설정하기
date: 2013-12-20T10:16:47+09:00
categories:
  - GIT
tags:
  - .gitignore
  - GIT
---

git을 이용할 때 무시하고 싶은 파일이 있을 때에는 .gitignore 파일을 작성하여 사용한다.

.gitignore 파일을 작성하는 방법 : <http://emflant.tistory.com/127>

.gitignore 파일을 작성하는건 어렵지 않은데 이것을 적용하니까 이미 .gitignore에 무시하도록 설정한 파일이 자꾸 나타났다. 이 문제를 해결하려면 다음과 같이 처리한다. (참고 : <http://blog.naver.com/deersoo?Redirect=Log&logNo=70169497582>)

git으로 관리하고 있는 프로젝트의 루트로 이동한다.

```console
git rm -r --cached .

git add .

git commit -a
```

`git rm -r --cached .` 은 git의 삭제명령인데 -r은 하위폴더도 모두 삭제하는 옵션이고 --cached는 git 시스템이 인덱스하고 있는 부분을 삭제하라는 의미이다. 이 명령으로 캐시를 다 삭제하고 다시 add 한다음 commit 하고 push를 하면 그다음부터는 .gitignore에 있던 파일들은 안 나타나게 된다.
