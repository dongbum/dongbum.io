---
title: CentOS 7에 plexpy 설치
date: 2017-09-13T00:33:46+09:00
categories:
  - Linux
tags:
  - CentOS
  - linux
  - PLEX
  - plexpy
  - 리눅스
  - 서버
---
plex 서버를 잘 사용 중인데 plexpy라는게 있다고 한다. 대강 보니 모니터툴 같은데 일단 리눅스에서 plexpy 를 설치해본다. 인터넷에 있는 몇몇 문서들은 도커를 이용한 설치를 예시로 하고 있는데 내 서버들은 아직 도커를 쓰지 않으므로 그냥 설치하는 것으로.

리눅스는 CentOS 7. plexpy 홈페이지에 들어가 InstallGuide 문서를 읽어본다.

<https://github.com/JonnyWong16/plexpy/wiki/Installation>

GIT을 이용하여 소스 코드를 다운 받는다.

```
[root@localhost source]# git clone https://github.com/JonnyWong16/plexpy.git
Cloning into 'plexpy'...
remote: Counting objects: 15652, done.
remote: Total 15652 (delta 0), reused 0 (delta 0), pack-reused 15652
Receiving objects: 100% (15652/15652), 77.18 MiB | 2.67 MiB/s, done.
Resolving deltas: 100% (8905/8905), done.
[root@localhost source]#
```

설치문서에는 /opt 에 설치하는 것을 예로 들고 있지만 이 경로를 난 좋아하지 않으므로 /usr/local/plexpy 에 설치할 예정.

다운 받은 내용을 /usr/local/plexpy 로 이동한다.

```
[root@localhost source]# mv plexpy /usr/local/
[root@localhost source]# cd /usr/local/plexpy
```

plexpy.py를 실행하면 StandAlone 으로 실행하는 것 같지만 난 데몬서비스가 더 좋으므로 관련 문서를 읽어본다.

<https://github.com/JonnyWong16/plexpy/wiki/Install-as-a-daemon>

리눅스인 경우 다음의 문서를 읽어보라고 한다.

<https://github.com/JonnyWong16/plexpy/blob/master/init-scripts/init.systemd>

보여지는 스크립트 파일 내용을 vi 를 이용해 plexpy.service 파일로 입력한다.

파일 안에 있는 /opt 로 시작하는 경로들을 나에게 맞도록 /usr/local q로 시작하도록 수정한 다음 저장한다. 실행 유저과 그룹도 plexpy 로 변경.

.service 파일은 보통 /lib/systemd/system 에 저장한다고 하니 그곳으로 옮겨주고 스크립트를 재로딩한 다음, 제대로 읽혀지는지 테스트해본다.

```
[root@localhost plexpy]# mv plexpy.service /lib/systemd/system
[root@localhost plexpy]# systemctl daemon-reload
[root@localhost plexpy]# systemctl status plexpy.service
● plexpy.service - PlexPy - Stats for Plex Media Server usage
Loaded: loaded (/usr/lib/systemd/system/plexpy.service; disabled; vendor preset: disabled)
Active: inactive (dead)
[root@localhost plexpy]#
```

서비스를 시작한다.

```
[root@localhost plexpy]# systemctl start plexpy.service
Job for plexpy.service failed because the control process exited with error code. See "systemctl status plexpy.service" and "journalctl -xe" for details.
[root@localhost plexpy]#
```

에러가 난다. 에러메시지대로 status 명령을 입력해본다.

```
[root@localhost plexpy]# systemctl status plexpy.service
 ● plexpy.service - PlexPy - Stats for Plex Media Server usage
 Loaded: loaded (/usr/lib/systemd/system/plexpy.service; disabled; vendor preset: disabled)
 Active: failed (Result: exit-code) since 화 2017-09-12 14:24:11 KST; 1min 10s ago
 Process: 23144 ExecStart=/usr/local/plexpy/PlexPy.py --quiet --daemon --nolaunch --config /usr/local/plexpy/config.ini --datadir /usr/local/plexpy (code=exited, status=217/USER)

9월 12 14:24:11 localhost.localdomain systemd[1]: Starting PlexPy - Stats for Plex Media Server usage...
9월 12 14:24:11 localhost.localdomain systemd[1]: plexpy.service: control process exited, code=exited status=217
9월 12 14:24:11 localhost.localdomain systemd[1]: Failed to start PlexPy - Stats for Plex Media Server usage.
9월 12 14:24:11 localhost.localdomain systemd[1]: Unit plexpy.service entered failed state.
9월 12 14:24:11 localhost.localdomain systemd[1]: plexpy.service failed.
[root@localhost plexpy]#
```

설치 문서를 읽어보니 환경설정에 대한 내용을 하나도 실행 안한 것이었다.

유저를 추가하고 권한을 준다.

```
[root@localhost plexpy]# adduser --system --no-create-home plexpy
[root@localhost plexpy]# chown plexpy:plexpy -R /usr/local/plexpy
```

이제 다시 시작해보면 에러가 나지 않는다.

```
[root@localhost plexpy]# systemctl start plexpy.service
```

서비스를 자동실행으로 등록한다.

```
[root@localhost plexpy]# systemctl enable plexpy.service
Created symlink from /etc/systemd/system/multi-user.target.wants/plexpy.service to /usr/lib/systemd/system/plexpy.service.
[root@localhost plexpy]#
```

웹브라우저로 서버의 8181 포트를 열어보면 다음과 같은 화면이 나온다. 이제부터는 웹에서 설정!

![](/assets/images/pleypy.png)

그런데 막상 설치해서 보니까 이게 뭐... 딱히 모니터링이라 하기도 애매하고... 어디다 써야할지는 잘 모르겠다. 아파치에 같이 물릴려고 했는데 귀찮아서 그냥 쓰는 것으로.
