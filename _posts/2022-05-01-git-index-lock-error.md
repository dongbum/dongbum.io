---
title: GIT에서 index lock 에러 메시지
date: 2022-05-01T01:00:36+09:00
categories:
  - GIT
tags:
  - GIT
  - Jenkins
---

젠킨스에서 빌드를 하고 있는데 이때 Perforce 에서 소스코드를 가져오고 빌드를 한 다음 git 으로 commin 하고 push 하는 과정을 거치고 있습니다.

하루는 빌드에 계속 문제가 생겨서 살펴보던 중 젠킨스에서 다음의 로그를 발견하였습니다.

```
Locking support detected on remote "origin". Consider enabling it with:
  $ git config lfs.https://aaa.com/project.git/info/lfs.locksverify true
To https://aaa.com/project.git/server.git
 * [new tag]           20220412-01 -> 20220412-01
fatal: Unable to create 'D:/aaa/git/server/.git/index.lock': File exists.

Another git process seems to be running in this repository, e.g.
an editor opened by 'git commit'. Please make sure all processes
are terminated then try again. If it still fails, a git process
may have crashed in this repository earlier:
remove the file manually to continue.
fatal: Unable to create 'D:/aaa/git/server/.git/index.lock': File exists.

Another git process seems to be running in this repository, e.g.
an editor opened by 'git commit'. Please make sure all processes
are terminated then try again. If it still fails, a git process
may have crashed in this repository earlier:
remove the file manually to continue.
Everything up-to-date
```

그런데 이렇게 에러가 난다해도 젠킨스의 프로젝트 빌드 결과는 Success 이기 때문에 에러라고 알아차릴 수 없었기에 문제 상황을 알아내는데 시간이 꽤 걸렸습니다.

여튼 위처럼 에러메시지가 나올 때는 index.lock 파일을 삭제해서 해결할 수 있습니다.

### 참고자료
* <https://jinseongsoft.tistory.com/151>
