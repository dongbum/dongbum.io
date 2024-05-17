---
title: Redis 사용시 SET 이 에러나거나 Protected Mode 가 활성화 될 때 해결방법
date: 2016-05-17T15:16:46+09:00
categories:
  - Database/SQL
  - Programming
tags:
  - Redis
  - SET
---
Redis를 캐시서버로 쓰고자 했을 때 SET이 되지 않았다. GET은 되는데 SET만 안되는 특이한 현상.

레디스 클라이언트 라이브러리 제작자가 첨부한 예제도 작동하지 않아서 C++에서 발생한 에러메시지를 살피던 중 다음과 같은 에러 메시지를 발견했다.

> SET error: DENIED Redis is running in protected mode because protected mode is enabled, no bind address was specified, no authentication password is requested to clients. In this mode connections are only accepted from the loopback interface. If you want to connect from external computers to Redis you may adopt one of the following solutions: 1) Just disable protected mode sending the command 'CONFIG SET protected-mode no' from the loopback interface by connecting to Redis from the same host the server is running, however MAKE SURE Redis is not publicly accessible from internet if you do so. Use CONFIG REWRITE to make this change permanent. 2) Alternatively you can just disable the protected mode by editing the Redis configuration file, and setting the protected mode option to 'no', and then restarting the server. 3) If you started the server manually just for testing, restart it with the '--protected-mode no' option. 4) Setup a bind address or an authentication password. NOTE: You only need to do one of the above things in order for the server to start accepting connections from the outside.

처음 보는 이 메시지에 당황해서 찾아보았다.

해결방법은 레디스 프로그램 파일에 첨부된 redis.conf 에 있었다. 이 파일에 보면 protected-mode 라는 환경설정 항목이 있고 이 항목의 값은 yes 로 되어 있다. 이 protected-mode 지시자가 무엇인지는 위에 주석에 자세히 써있다.

> # Protected mode is a layer of security protection, in order to avoid that
> # Redis instances left open on the internet are accessed and exploited.
> #
> # When protected mode is on and if:
> #
> # 1) The server is not binding explicitly to a set of addresses using the
> # "bind" directive.
> # 2) No password is configured.
> #
> # The server only accepts connections from clients connecting from the
> # IPv4 and IPv6 loopback addresses 127.0.0.1 and ::1, and from Unix domain
> # sockets.
> #
> # By default protected mode is enabled. You should disable it only if
> # you are sure you want clients from other hosts to connect to Redis
> # even if no authentication is configured, nor a specific set of interfaces
> # are explicitly listed using the "bind" directive.

대략적인 내용은 보호모드라는 항목이 있으며 이것은 인터넷을 통한 접속과 부당한 이용을 막기 위함이라고 한다. 이 보호모드를 다음과 같은 경우에 켜라고 한다. 서버가 bind 지시어를 통해 바인딩되어 있지 않은 경우, 암호가 설정되지 않은 경우.

여튼 보호모드라는게 문제이니 이 문제를 해결하려면 두가지가 있는데 bind 지시어에 서버의 외부아이피 주소를 지정하거나 혹은 보호모드를 yes 에서 no 로 변경해주면 된다.

변경하고나면 SET이 잘되기 시작한다.

예전에 윈도우용 레디스 2.8 버전을 쓸 때 이러한 모드가 없었는데 최근 3.0 버전에 생긴건가...?
