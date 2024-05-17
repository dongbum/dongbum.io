---
title: svn2git으로 svn 데이터를 git으로 마이그레이션
categories:
  - GIT
tags:
  - GIT
  - svn
  - svn2git
last_modified_at: 2019-10-15T14:38:39+09:00
---
SVN 데이터를 GIT으로 가져가기 위한 방법. `svn2git` 이라는 프로젝트를 이용한다.

---

### svn2git 설치

먼저 svn과 git이 설치되어 있어야 한다. 또한 git-svn 도 설치되어 있어야 한다.

svn 이 프로젝트는 루비로 되어있으므로 일단 ruby부터 설치. 우분투에서는

```console
apt-get install ruby
```

입력.

설치 후

```console
gem install svn2git
```

을 입력하여 svn2git을 설치한다.

주의할 점이 이 모든 것들이 리눅스에서 이루어져야한다. 이유는 아래에.

---

### 커미터(작성자) 목록 만들기

svn에서 코드 커밋을 했던 사람들을 git에 있는 커미터로 매칭해서 변경해줘야한다.

참고자료에 있는 내용처럼 .svn2git 폴더를 생성한다.

svn 레포지토리에서 커미터리스트를 뽑으려면 다음의 명령을 실행하면 된다.

```console
svn log|grep "^r[0-9]"|awk -F\| '{print $2}'| sed -e 's/^[ \t]*//'|sort|uniq
```

문제는 이 명령줄에 쓰인 grep, awk 등은 리눅스에 있는 명령어들이며 윈도우에서는 없는 명령들이다. 이 명령에 해당하는 부분을 직접 구현하던가 아니면 커미터리스트를 일일치 찾아서 파일을 작성해야한다.

이 명령을 수행하면 커미터 리스트가 나오고 그것을 authors 파일로 저장한다. authors 파일에는 각 아이디에 해당하는 유저이름과 이메일까지 매칭해서 작성해야 한다.

---

### SVN2GIT 명령 수행시 특정 확장자 제거

비주얼스튜디오로 만들어진 프로젝트를 처리하다보면 exe, map, pdb 파일등이 포함되어 있는데 다음과 같은 명령을 통해 이런 파일들을 거를 수 있다.

```console
svn2git http://repository.com --exclude '([^\s]+(?=\.(exe|map|pdb))\.\2)'
```

**실제로 해보니 제대로 처리되지 않았다. exclude 옵션이 작동하지 않는듯.**

---

### 참고

  * <https://www.lesstif.com/pages/viewpage.action?pageId=23757066>
