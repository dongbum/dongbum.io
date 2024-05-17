---
title: Visual Studio에서 jemalloc 설치하고 사용법 (1)
date: 2018-02-07T15:51:24+09:00
categories:
  - 'C#'
tags:
  - C
  - jemalloc
  - tcmalloc
---
> 이 글은 jemalloc 을 설치하다가 애먹은 경험으로 쓰는 것. 혹시 나처럼 Visual Studio에서 jemalloc을 쓰려고 고생하는 사람들에게 도움이 되길 바란다. 그리고 퍼갈 때에는 출처도 밝혀주시기를...

tcmalloc 이 VIsual Studio 2015 에서 작동이 제대로 안된다는 것을 알고 난 이후 다른 대안을 찾아보았다. 비슷한 기능을 하는 것으로 jemalloc과 nedmalloc가 있었는데 nedmalloc은 github에서 보니 개발이 멈춘지가 한참전이었다. 몇년전부터 개발이 중지된 것을 쓰기는 싫기에 jemalloc을 다운받았다.

jemalloc 에 대한 성능이나 기술적인 이야기는 [마지막 남은 공짜 점심. Facebook의 메모리 할당자 jemalloc](http://channy.creation.net/project/dev.kthcorp.com/2011/05/12/last-free-lunch-facebooks-memory-allocator-jemalloc/index.html)라는 글에 잘 설명되어 있다. 한번 읽어보는 것도 괜찮겠다.

참고로 jemalloc 이 현재 쓰이는 곳은 github의 [IntendedUse](https://github.com/jemalloc/jemalloc/wiki/Background#intended-use) 항목에서 찾아볼 수 있다.

  * [FreeBSD](http://www.freebsd.org/)
  * [NetBSD](http://www.netbsd.org/)
  * [Mozilla Firefox](http://www.mozilla.org/firefox/)
  * [Varnish](https://www.varnish-cache.org/)
  * [Cassandra](http://cassandra.apache.org/)
  * [Redis](http://redis.io/)
  * [hhvm](https://github.com/facebook/hiphop-php)
  * [MariaDB](https://mariadb.org/)
  * [Aerospike](http://www.aerospike.com/)
  * [Android](https://github.com/android/platform_bionic)

에서 쓰이고 있다고 한다. 개발자라면 대부분 알만한 유명한 프로젝트들.
