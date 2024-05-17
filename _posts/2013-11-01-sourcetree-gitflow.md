---
title: SourceTree와 함께 Git과 git-flow 사용해보기
date: 2013-11-01T01:00:57+09:00
categories:
  - GIT
  - Linux
  - Programming
tags:
  - GIT
  - git-flow
  - SourceTree
---
최근 회사에서 Subversion에서 Git으로 이전하고 있고 나 역시도 이제부터는 Git으로 옮겨가려는 생각이라 내 서버에 Git을 설치하고 사용해보기로 한다.

서버에는 yum으로 Git을 설치했다. 몇번의 시행착오끝에 방법을 알게되어 기록해놓는다.

서버에 저장소를 만들려면 다음의 순서를 따른다.

  1. 저장소 레포지토리를 만들 위치에 가서 mkdir로 디렉토리를 만든다. 일반적으로는 '프로젝트이름.git'을 사용하는듯하다.
  2. 만든 디렉토리롤 들어가서 'git init --bare'를 입력하여 초기화한다.

여기까지가 서버상의 설정이다. 물론 권한등을 더 설정해야할 수도 있다.

이제 SourceTree 어플리케이션을 사용해본다.

  1. SourceTree에서 저장소를 설정하여 연동한다.
  2. Git은 아무 파일도 없다면 master 브랜치가 생성되지 않는다고 한다. 로컬컴퓨터의 working copy 디렉토리에 touch 명령어로 빈파일을 하나 생성한다.
  3. SourceTree에 가서 commit을 한다.
  4. master 브랜치가 생성되을 것이므로 SourceTree의 git-flow 버튼을 눌러서 초기화하겠다는 창이 뜨면 OK를 눌러서 초기화한다.
  5. 이제 git-flow를 사용할 준비 완료.

몇번의 삽질 끝에 대략 이러한 과정을 거치면 된다는 것을 알았다. 나머지 레드마인과의 연동 등은 다음에 정리해보는 것으로.
