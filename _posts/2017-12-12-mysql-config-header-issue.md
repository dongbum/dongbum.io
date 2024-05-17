---
title: MySQL 라이브러리 사용시 config.h 파일 문제
date: 2017-12-12T12:02:26+09:00
categories:
  - boost
  - C/C++
  - Database/SQL
---
리눅스에서 빌드시 다음과 같은 오류가 있었다.

```
In file included from /root/DServer2/src/dserver/protocol/../../dserver/database.h:6:0,
from /root/DServer2/src/dserver/protocol/../../dserver/define.h:41,
from /root/DServer2/src/dserver/protocol/base_protocol.h:3,
from /root/DServer2/src/dserver/protocol/base_protocol.cpp:1:
/root/Library/mysql-connector-c++-1.1.9/cppconn/resultset.h:30:20: fatal error: config.h: 그런 파일이나 디렉터리가 없습니다
#include "config.h"
^
```

config.h 파일이 없다는 에러 메시지인데 cppconn 디렉토리를 살펴보면 config.h.cm 파일이 있다.

mysql 커넥터를 다운 받은 디렉토리로 가서 cmake를 실행한다. 실행이 되지 않는다면 yum 으로 cmake를 설치한다.

```
[root@localhost Library]# cmake mysql-connector-c++-1.1.9
-- The C compiler identification is GNU 4.8.5
-- The CXX compiler identification is GNU 4.8.5
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Could NOT find Boost
-- Could NOT find Boost
CMake Error at CMakeLists.txt:194 (MESSAGE):
Boost or some of its libraries found. If not in standard place please set
-DBOOST_ROOT:STRING=


-- Configuring incomplete, errors occurred!
See also "/root/Library/CMakeFiles/CMakeOutput.log".
[root@localhost Library]#
```

<https://dev.mysql.com/doc/connector-cpp/en/connector-cpp-installation-source-unix.html>를 참조했다.

<https://dev.mysql.com/doc/connector-cpp/en/connector-cpp-source-configuration-options.html>를 읽어봤더니 BOOST 경로를 설정해줘야한다.

`/etc/profile` 에 BOOST_ROOT 경로를 지정한다.

```
export BOOST_ROOT=/부스트경로/
```

다시 cmake를 실행했다.

```
[root@localhost mysql-connector-c++-1.1.9]# cmake .
-- Boost version: 1.65.1
-- BOOST_INCLUDE_DIRS=/root/Library/boost_1_65_1
-- You will link dynamically to the MySQL client library (set with -DMYSQLCLIENT_STATIC_LINKING=<bool>)
-- Searching for dynamic libraries with the base name(s) "mysqlclient_r mysqlclient"
CMake Error at FindMySQL.cmake:531 (message):
Could not find "mysql.h" from searching "/usr/include/mysql
/usr/local/include/mysql /opt/mysql/mysql/include
/opt/mysql/mysql/include/mysql /usr/local/mysql/include
/usr/local/mysql/include/mysql /MySQL/*/include /MySQL/*/include"
Call Stack (most recent call first):
CMakeLists.txt:252 (INCLUDE)


-- Configuring incomplete, errors occurred!
See also "/root/Library/mysql-connector-c++-1.1.9/CMakeFiles/CMakeOutput.log".
[root@localhost mysql-connector-c++-1.1.9]#
```

mysql 라이브러리 파일이 있어야한다고 나온다. CentOS 7에서는 MySQL이 빠지고 그대신 MariaDB가 들어가 있기 때문에 mariadb-libs 와 mariadb-devel 을 yum으로 설치해준다.

다시 cmake 하니...

```
# cmake .
-- Boost version: 1.65.1
-- BOOST_INCLUDE_DIRS=/root/Library/boost_1_65_1
-- You will link dynamically to the MySQL client library (set with -DMYSQLCLIENT_STATIC_LINKING=<bool>)
-- Searching for dynamic libraries with the base name(s) "mysqlclient_r mysqlclient"
-- mysql_config was found /usr/bin/mysql_config
-- MySQL client environment/cmake variables set that the user can override
--   MYSQL_DIR                   :
--   MYSQL_INCLUDE_DIR           : /usr/include/mysql
--   MYSQL_LIB_DIR               : /usr/lib64/mysql
--   MYSQL_CONFIG_EXECUTABLE     : /usr/bin/mysql_config
--   MYSQL_CXX_LINKAGE           :
--   MYSQL_CFLAGS                : -I/usr/include/mysql
--   MYSQL_CXXFLAGS              : -I/usr/include/mysql

(...중략...)

-- Configuring performance test - statement
-- Configuring bugs test cases - unsorted
-- Configuring unit tests - group template_bug
-- Configuring done
CMake Warning (dev) in driver/CMakeLists.txt:
  Policy CMP0022 is not set: INTERFACE_LINK_LIBRARIES defines the link
  interface.  Run "cmake --help-policy CMP0022" for policy details.  Use the
  cmake_policy command to set the policy and suppress this warning.

  Target "mysqlcppconn" has an INTERFACE_LINK_LIBRARIES property which
  differs from its LINK_INTERFACE_LIBRARIES properties.

  INTERFACE_LINK_LIBRARIES:

    mysqlclient;pthread;z;m;ssl;crypto;dl

  LINK_INTERFACE_LIBRARIES:



This warning is for project developers.  Use -Wno-dev to suppress it.

-- Generating done
-- Build files have been written to: /root/Library/mysql-connector-c++-1.1.9
[root@localhost mysql-connector-c++-1.1.9]#
```

이제서야 완료. config.h 파일이 정상적으로 생성되었다.
