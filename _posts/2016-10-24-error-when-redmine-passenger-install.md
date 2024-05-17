---
title: redmine 설치 + passenger 모듈 설치시 에러
date: 2016-10-24T13:24:03+09:00
categories:
  - Linux
tags:
  - Passenger
  - Ruby
  - 레드마인
  - 리눅스
---
새로 설치된 서버에서 레드마인을 설치하다가 알게 된 사실.

레드마인에 Passenger 모듈을 이용해 아파치와 연동하려고 했다.

패신저 모듈을 설치하고 명령을 내렸는데...

```
c++ -o buildout/support-binaries/UstRouterMain.o  -Isrc/agent -Isrc/cxx_supportlib -Isrc/cxx_supportlib/vendor-copy -Isrc/cxx_supportlib/vendor-modified -Isrc/cxx_supportlib/vendor-modified/libev -Isrc/cxx_supportlib/vendor-copy/libuv/include -D_REENTRANT -I/usr/local/include -Wall -Wextra -Wno-unused-parameter -Wno-parentheses -Wpointer-arith -Wwrite-strings -Wno-long-long -Wno-missing-field-initializers -feliminate-unused-debug-symbols -feliminate-unused-debug-types -fvisibility=hidden -DVISIBILITY_ATTRIBUTE_SUPPORTED -Wno-attributes -ggdb -DHAS_ALLOCA_H -DHAVE_ACCEPT4 -DHAS_SFENCE -DHAS_LFENCE -DPASSENGER_DEBUG -DBOOST_DISABLE_ASSERTS -std=gnu++11 -Wno-unused-local-typedefs -DHASH_NAMESPACE="__gnu_cxx" -DHASH_MAP_HEADER="<hash_map>" -DHASH_MAP_CLASS="hash_map" -DHASH_FUN_H="<hash_fun.h>" -c src/agent/UstRouter/UstRouterMain.cpp
c++: internal compiler error: 죽었음 (program cc1plus)
Please submit a full bug report,
with preprocessed source if appropriate.
See <http://bugzilla.redhat.com/bugzilla> for instructions.
rake aborted!
-----------------------------------------------
Your compiler failed with the exit status 4. This probably means that it ran out of memory. To solve this problem, try increasing your swap space: https://www.digitalocean.com/community/articles/how-to-add-swap-on-ubuntu-12-04
/usr/local/share/gems/gems/passenger-5.0.30/build/support/cplusplus.rb:41:in `run_compiler'
/usr/local/share/gems/gems/passenger-5.0.30/build/support/cplusplus.rb:102:in `compile_cxx'
/usr/local/share/gems/gems/passenger-5.0.30/build/support/cplusplus.rb:160:in `block in define_cxx_object_compilation_task'
Tasks: TOP => apache2 => buildout/support-binaries/PassengerAgent => buildout/support-binaries/UstRouterMain.o
(See full trace by running task with --trace)

--------------------------------------------

It looks like something went wrong
```

메시지와 함께 에러. 메모리가 부족하니 스왑메모리 용량을 늘리라는 메시지인데 스왑메모리 용량을 늘리는 작업을 하기에는 너무나 번거롭고... 어떻게 해야하나 고민중. Thin 서버를 쓰는 방향을 생각 중이다.

추가.

`passenger-install-apache2-module` 명령을 내려서 실행하다보면 다음과 같은 메시지가 있다.

```
Your system does not have a lot of virtual memory

Compiling Phusion Passenger works best when you have at least 1024 MB of virtual
memory. However your system only has 993 MB of total virtual memory (993 MB
RAM, 0 MB swap). It is recommended that you temporarily add more swap space
before proceeding. You can do it as follows:

  sudo dd if=/dev/zero of=/swap bs=1M count=1024
  sudo mkswap /swap
  sudo swapon /swap

See also https://wiki.archlinux.org/index.php/Swap for more information about
the swap file on Linux.

If you cannot activate a swap file (e.g. because you're on OpenVZ, or if you
don't have root privileges) then you should install Phusion Passenger through
DEB/RPM packages. For more information, please refer to our installation
documentation:

  https://www.phusionpassenger.com/library/install/apache/

Press Ctrl-C to abort this installer (recommended).
Press Enter if you want to continue with installation anyway.
```

읽어보면 메모리가 부족하다는 경고와 메모리 부족시 해결하는 방법과 작업실행 중단을 권고하는 메시지이다. 처음에는 이 메시지를 무시하고 계속 설치했기에 문제가 일어났었다.

구글에 검색해봐도 뾰족한 방법이 없기에 안내 메시지에 따라서 실행해봤다.

```
[root@localhost redmine-3.3.1]# free -h
              total        used        free      shared  buff/cache   available
Mem:           993M        566M         80M         56M        346M        233M
Swap:            0B          0B          0B
[root@localhost redmine-3.3.1]# ll /dev/zero
crw-rw-rw- 1 root root 1, 5 10월 18 15:42 /dev/zero
[root@localhost redmine-3.3.1]# dd if=/dev/zero of=/swap bs=1M count=1024
1024+0 records in
1024+0 records out
1073741824 bytes (1.1 GB) copied, 2.11671 s, 507 MB/s
[root@localhost redmine-3.3.1]# mkswap /swap
Setting up swapspace version 1, size = 1048572 KiB
no label, UUID=0678c873-6899-4c19-b208-e2d69ed55bac
[root@localhost redmine-3.3.1]# swapon /swap
swapon: /swap: insecure permissions 0644, 0600 suggested.
[root@localhost redmine-3.3.1]# free -h
              total        used        free      shared  buff/cache   available
Mem:           993M        566M         66M         56M        359M        235M
Swap:          1.0G          0B        1.0G
[root@localhost redmine-3.3.1]#
```

실행하고 나니 0바이트였던 스왑메모리가 1기가로 지정되었다. 다시 `passenger-install-apache2-module` 명령을 내리면 아까와 같은 경고메시지는 없어지고 컴파일이 순조롭게 진행되었다.
