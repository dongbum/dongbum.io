---
title: PLEX에서 다음검색에이전트 설치
date: 2015-12-18T13:47:48+09:00
categories:
  - MicroServer
tags:
  - CentOS
  - linux
  - PLEX
  - 다음
  - 리눅스
  - 에이전트
---
PLEX 서버 설치 후 영화 추가시 데이터를 자동으로 가져오기 위해 다음검색 에이전트를 설치했다.

<https://forums.plex.tv/discussion/comment/486302>

에서 다음 검색 에이전트를 다운로드한다.

<https://github.com/axfree/DaumMovie.bundle> 에서 master.zip 파일을 다운로드 받을 수 있다. 리눅스에서는 wget 명령어를 이용하여 한번에 받을 수 있으므로 더 편리하다.

다운로드 파일을 `unzip master.zip` 명령으로 압축을 풀고, 폴더명을 DaumMovie.bundle-master 에서 DaumMovie.bundle 로 변경한다. (폴더명을 바꾸지 않으면 PLEX 서버에서 인식되지 않으므로 반드시 변경해야한다.

변경한 후 PLEX 서버를 `systemctl restart plexmediaserver` 명령으로 재시작해준다.

다시 웹관리자에 접속하여 설정 -> 서버 -> 에이전트 항목에 가보면 Daum Movie 라는 항목이 생겨있다.

![](/assets/images/daum-movie-agent.png)

밑의 Local Media Asstes (Movies) 의 체크박스를 활성화해주면 끝. (이게 되어있지 않다면 자막이 나오지 않는다고 한다.)

나머지 TV 쇼나 아티스트 앨범에 가서도 같은 작업을 반복해주면 된다.

이 작업을 하고 나면 영화나 TV쇼가 추가되면 자동으로 데이터를 가져오게 된다.
