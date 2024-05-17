---
title: 'docker 시작시  에러'
date: 2018-04-02T14:04:29+09:00
categories:
  - Docker
tags:
  - docker
  - 도커
  - 정전
  - 컨테이너
---
집에 갑자기 정전이 와서 서버가 꺼져버렸다.

서버가 다시 살아나고 도커로 사용하던 컨테이너들을 재시작하려고 하니 에러가 났다.

```
[root@localhost ~]# docker start plex
Error response from daemon: error creating overlay mount to /var/lib/docker/overlay2/40a78337fb7041d7de6ccd93467be6ec60f7baf325f062c97e24cc4d01a13d91/merged: invalid argument
Error: failed to start containers: plex
[root@localhost ~]#
```

이 문제를 해결하기 위해 구글 검색을 해보았으나 딱히 좋은 의견은 보질 못했다.

`docker container list` 명령으로 컨테이너를 찾아보았으나 보이지 않았다. `docker ps` 명령으로 찾아서 모든 컨테이너를 삭제한다.

```
[root@localhost docker]# docker ps -a
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
7d613c08b1f2 linuxserver/tautulli "/init" 7 days ago Exited (255) 3 hours ago tautulli
fdcb9b590e94 linuxserver/transmission "/init" 8 days ago Exited (255) 3 hours ago transmission
246a6d8a721b plexinc/pms-docker "/init" 10 days ago Exited (255) 3 hours ago plex
3040fb42bd81 oznu/homebridge "/init" 12 days ago Exited (255) 3 hours ago homebridge
[root@localhost docker]# docker container rm 7d613c08b1f2
7d613c08b1f2
[root@localhost docker]# docker container rm fdcb9b590e94
fdcb9b590e94
[root@localhost docker]# docker container rm 246a6d8a721b
246a6d8a721b
[root@localhost docker]# docker container rm 3040fb42bd81
3040fb42bd81
```

컨테이너를 모두 삭제하고 다시 컨테이너 실행스크립트를 이용해 새로운 컨테이너를 만들어서 실행시키니 해결되었다.

**정전 같은 이유로 불완전하게 종료되었을 경우, 도커의 재실행에 문제가 생긴다.** 이를 해결하는 방법 또한 시원하게 나온게 없었다. **컨테이너가 제대로 실행되지 않는다면 다 삭제하고 다시 실행하는게 차라리 빠르다.**

**귀찮은 실행스크립트를 다시 작성하지 않도록 잘 보관해야 한다.**
