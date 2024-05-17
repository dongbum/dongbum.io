---
title: git flow 에서 hotfix 사용 방법
date: 2013-12-17T14:44:41+09:00
categories:
  - GIT
tags:
  - GIT
  - git-flow
  - hotfix
---
git flow 에 익숙해져 가고 있는데 hotfix 사용법에 익숙치 않아서 애를 먹었다.

나중에 잊어버리지 않기 위해 hotfix 사용방법을 여기에 기록해둔다.

```
git flow hotfix start <핫픽스버전이름> <베이스버전이름>
```

명령으로 origin의 특정 브랜치로부터 핫픽스 브랜치를 시작한다.

수정작업을 진행한다.

수정작업을 진행하며 git commit 명령으로 계속 커밋을 해놓는다.

저장소로 커밋 내용을 보내기 위해

```
git push -u hotfix/<핫픽스버전이름>:hotfix/<핫픽스버전이름>
```

을 입력해서 저장소의 hotfix/<핫픽스버전이름> 브랜치로 계속 push를 한다.

수정 작업이 마무리되고 모든 것이 끝났다면 로컬의 develop 브랜치로 이동한 다음, git pull 명령을 입력하여 다른 사람들이 작업했던 것들을 받아온다.

```
git flow hotfix finish <핫픽스버전이름>
```

명령을 사용하여 핫픽스를 종료하고 hotfix 브랜치를 삭제하고 develop 브랜치에 변경 내용을 병합한다. 충돌이 나면 수정한다.

git push 명령으로 저장소에 반영한다.
