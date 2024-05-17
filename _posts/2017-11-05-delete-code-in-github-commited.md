---
title: Github 에서 내용에서 민감한 정보 삭제하기
date: 2017-11-05T23:41:27+09:00
categories:
  - GIT
tags:
  - GIT
  - github
---
![](/assets/images/github-card.png)

github에 소스코드를 올리면서 중요한 코드를 삭제하지 않고 올렸다는걸 나중에서야 알게되었다. github에서 코드를 지워야하는 상황.

구글에 검색해보니 몇가지 답이 나왔다. 명령어를 입력해서 처리하는 방법과 bfg라는 툴을 이용하는 방법 두가지가 있는데 일단 bfg는 좀 귀찮았고(많은 설명들에서 bfg가 무엇인지 안나와있는데 github에서 bfg를 검색해보면 된다.) 명령어를 입력해서 처리해보기로 결정.

여튼 검색된 문서의 이런저런 방법대로 해봐도 잘 안되서 마지막은 github에서 공식적으로 안내된 문서대로 해보았더니 바로 적용되었다. (<https://help.github.com/articles/removing-sensitive-data-from-a-repository>)

일단 **삭제할 내용이 포함된 파일을 백업한 다음**, 입력할 명령어는 다음과 같다.

```
git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch 삭제될파일의_전체경로' --prune-empty --tag-name-filter cat -- --all
```

이것을 입력하고 일단 무언가 텍스트들이 잠시 지나가길 기다린 다음, 마지막으로 push를 하면 된다. 삭제될 파일의 경로는 정확하게 경로 모두와 파일명 전부를 적어줘야한다.

```
git push origin --force --all
```

그리고 나면 해당 파일이 삭제되어 있다. 백업했던 파일을 수정한 다음 원래 위치로 다시 복구하고, add, commit, push를 차례대로 해준다.

그러면 복구 완료.
