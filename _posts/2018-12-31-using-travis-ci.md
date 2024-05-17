---
title: Travis CI 사용
date: 2018-12-31T15:56:01+09:00
categories:
  - C/C++
  - GameServer
  - GIT
---
github를 사용하다가 얼마전부터 Travis CI 를 연동해서 사용해보기로 했다.

![](/assets/images/TravisCI-Full-Color.png)

외부 라이브러리를 전혀 쓰지 않게 되어있는 URLParser는 빌드에 어려움이 없었지만, 외부라이브러리를 가져다 쓰는 DServer는 빌드에 애로사항이 많았다.

빌드명령어는 <https://arne-mertz.de/2017/04/continuous-integration-travis-ci> 를 참조했다.

일단 나는 CentOS에서만 빌드하고 테스트했었기 때문에 Travis CI가 빌드에 사용하는 Ubuntu 리눅스에 맞추도록 해야했다.

일단 사용했던 라이브러리인 jemalloc 에서부터 에러가 났다. VS2015에 라이브러리를 빌드할 수 있는 모든 파일들이 포함되어 있긴했지만 이걸 또 빌드하려면 여러가지 명령을 넣어야하는 상황. 구글을 뒤져서 travis에서 빌드전 libjemalloc-dev 패키지를 설치하도록 추가했다.

다시 빌드하려고 하니 이번엔 json 패키지에서 에러가 났다. 역시 모든 파일이 포함되어 있지만 이걸 빌드하기에는 복잡하니 다시 라이브러리를 추가하도록 했다.

우분투에서 패키지 검색은

`sudo apt-cache search [패키지명]`

를 사용하면 된다.

빌드전 설치 패키지에 libjsoncpp-dev 를 추가한다. 이것만으로 안되서 libjson0 libjson0-dev 도 추가했다. 이제 다시 빌드하면...

기대도 안했지만 역시 에러난다. boost 관련 에러가 뜬다. 그런데 boost가 어떤 패키지에 담겨있는지 알수가 없는 상황.

<https://docs.travis-ci.com/user/build-environment-updates/2017-11-01>

를 보니 boost 1.65가 있다고는 한다. 결국에는 apt-cache search 명령으로 패키지 목록을 찾아냈다. 2018년 12월 26일 기준으로 boost 와 관련된 라이브러리 목록은 다음과 같다.

```
before_install.1
0.28s$ sudo apt-cache search boost
libboost-chrono-dev - C++ representation of time duration, time point, and clocks (default version)
libboost-date-time-dev - set of date-time libraries based on generic programming concepts (default version)
libboost-dbg - Boost C++ Libraries with debug symbols (default version)
libboost-dev - Boost C++ Libraries development files (default version)
libboost-doc - Boost.org libraries documentation (default version)
libboost-filesystem-dev - filesystem operations (portable paths, iteration over directories, etc) in C++ (default version)
libboost-iostreams-dev - Boost.Iostreams Library development files (default version)
libboost-program-options-dev - program options library for C++ (default version)
libboost-python-dev - Boost.Python Library development files (default version)
libboost-regex-dev - regular expression library for C++ (default version)
libboost-serialization-dev - serialization library for C++ (default version)
libboost-system-dev - Operating system (e.g. diagnostics support) library (default version)
libboost-test-dev - components for writing and executing test suites (default version)
libboost-thread-dev - portable C++ multi-threading (default version)
libboost-all-dev - Boost C++ Libraries development files (ALL) (default version)
libboost-atomic-dev - atomic data types, operations, and memory ordering constraints (default version)
libboost-atomic1.55-dev - atomic data types, operations, and memory ordering constraints
libboost-atomic1.55.0 - atomic data types, operations, and memory ordering constraints
libboost-chrono1.55-dev - C++ representation of time duration, time point, and clocks
libboost-chrono1.55.0 - C++ representation of time duration, time point, and clocks
libboost-context-dev - provides a sort of cooperative multitasking on a single thread (default version)
libboost-context1.55-dev - provides a sort of cooperative multitasking on a single thread
libboost-context1.55.0 - provides a sort of cooperative multitasking on a single thread
libboost-coroutine-dev - provides a sort of cooperative multitasking on a single thread (default version)
libboost-coroutine1.55-dev - provides a sort of cooperative multitasking on a single thread
libboost-date-time1.55-dev - set of date-time libraries based on generic programming concepts
libboost-date-time1.55.0 - set of date-time libraries based on generic programming concepts
libboost-exception-dev - library to help write exceptions and handlers (default version)
libboost-exception1.55-dev - library to help write exceptions and handlers
libboost-filesystem1.55-dev - filesystem operations (portable paths, iteration over directories, etc) in C++
libboost-filesystem1.55.0 - filesystem operations (portable paths, iteration over directories, etc) in C++
libboost-geometry-utils-perl - Perl module providing bindings to the Boost Geometry library
libboost-graph-dev - generic graph components and algorithms in C++ (default version)
libboost-graph-parallel-dev - generic graph components and algorithms in C++ (default version)
libboost-graph-parallel1.55-dev - generic graph components and algorithms in C++
libboost-graph-parallel1.55.0 - generic graph components and algorithms in C++
libboost-graph1.55-dev - generic graph components and algorithms in C++
libboost-graph1.55.0 - generic graph components and algorithms in C++
libboost-iostreams1.55-dev - Boost.Iostreams Library development files
libboost-iostreams1.55.0 - Boost.Iostreams Library
libboost-locale-dev - C++ facilities for localization (default version)
libboost-locale1.55-dev - C++ facilities for localization
libboost-locale1.55.0 - C++ facilities for localization
libboost-log-dev - C++ logging library (default version)
libboost-log1.55-dev - C++ logging library
libboost-log1.55.0 - C++ logging library
libboost-math-dev - Boost.Math Library development files (default version)
libboost-math1.55-dev - Boost.Math Library development files
libboost-math1.55.0 - Boost.Math Library
libboost-mpi-dev - C++ interface to the Message Passing Interface (MPI) (default version)
libboost-mpi-python-dev - C++ interface to the Message Passing Interface (MPI), Python Bindings (default version)
libboost-mpi-python1.55-dev - C++ interface to the Message Passing Interface (MPI), Python Bindings
libboost-mpi-python1.55.0 - C++ interface to the Message Passing Interface (MPI), Python Bindings
libboost-mpi1.55-dev - C++ interface to the Message Passing Interface (MPI)
libboost-mpi1.55.0 - C++ interface to the Message Passing Interface (MPI)
libboost-program-options1.55-dev - program options library for C++
libboost-program-options1.55.0 - program options library for C++
libboost-python1.55-dev - Boost.Python Library development files
libboost-python1.55.0 - Boost.Python Library
libboost-random-dev - Boost Random Number Library (default version)
libboost-random1.55-dev - Boost Random Number Library
libboost-random1.55.0 - Boost Random Number Library
libboost-regex1.55-dev - regular expression library for C++
libboost-regex1.55.0 - regular expression library for C++
libboost-serialization1.55-dev - serialization library for C++
libboost-serialization1.55.0 - serialization library for C++
libboost-signals-dev - managed signals and slots library for C++ (default version)
libboost-signals1.55-dev - managed signals and slots library for C++
libboost-signals1.55.0 - managed signals and slots library for C++
libboost-system1.55-dev - Operating system (e.g. diagnostics support) library
libboost-system1.55.0 - Operating system (e.g. diagnostics support) library
libboost-test1.55-dev - components for writing and executing test suites
libboost-test1.55.0 - components for writing and executing test suites
libboost-thread1.55-dev - portable C++ multi-threading
libboost-thread1.55.0 - portable C++ multi-threading
libboost-timer-dev - C++ wall clock and CPU process timers (default version)
libboost-timer1.55-dev - C++ wall clock and CPU process timers
libboost-timer1.55.0 - C++ wall clock and CPU process timers
libboost-tools-dev - Boost C++ Libraries development tools (default version)
libboost-wave-dev - C99/C++ preprocessor library (default version)
libboost-wave1.55-dev - C99/C++ preprocessor library
libboost-wave1.55.0 - C99/C++ preprocessor library
libboost1.55-all-dev - Boost C++ Libraries development files (ALL)
libboost1.55-dbg - Boost C++ Libraries with debug symbols
libboost1.55-dev - Boost C++ Libraries development files
libboost1.55-doc - Boost.org libraries documentation
libboost1.55-tools-dev - Boost C++ Libraries development tools
libjson-spirit-dev - C++ JSON Parser/Generator implemented with Boost Spirit
pianobooster - learn the piano just by playing a game
pianobooster-dbg - learn the piano just by playing a game - debug
python-minieigen - Small boost::python wrapper of parts of the Eigen library
python-py++ - OO-framework for creating a code generator for Boost.Python
r-cran-gbm - GNU R package "Generalized Boosted Regression Models"
libboost-atomic1.54-dev - atomic data types, operations, and memory ordering constraints
libboost-atomic1.54.0 - atomic data types, operations, and memory ordering constraints
libboost-chrono1.54-dev - C++ representation of time duration, time point, and clocks
libboost-chrono1.54.0 - C++ representation of time duration, time point, and clocks
libboost-date-time1.54-dev - set of date-time libraries based on generic programming concepts
libboost-date-time1.54.0 - set of date-time libraries based on generic programming concepts
libboost-filesystem1.54-dev - filesystem operations (portable paths, iteration over directories, etc) in C++
libboost-filesystem1.54.0 - filesystem operations (portable paths, iteration over directories, etc) in C++
libboost-iostreams1.54-dev - Boost.Iostreams Library development files
libboost-iostreams1.54.0 - Boost.Iostreams Library
libboost-program-options1.54-dev - program options library for C++
libboost-program-options1.54.0 - program options library for C++
libboost-python1.54-dev - Boost.Python Library development files
libboost-python1.54.0 - Boost.Python Library
libboost-regex1.54-dev - regular expression library for C++
libboost-regex1.54.0 - regular expression library for C++
libboost-serialization1.54-dev - serialization library for C++
libboost-serialization1.54.0 - serialization library for C++
libboost-system1.54-dev - Operating system (e.g. diagnostics support) library
libboost-system1.54.0 - Operating system (e.g. diagnostics support) library
libboost-test1.54-dev - components for writing and executing test suites
libboost-test1.54.0 - components for writing and executing test suites
libboost-thread1.54-dev - portable C++ multi-threading
libboost-thread1.54.0 - portable C++ multi-threading
libboost1.54-dbg - Boost C++ Libraries with debug symbols
libboost1.54-dev - Boost C++ Libraries development files
libboost1.54-doc - Boost.org libraries documentation
libboost1.54-tools-dev - Boost C++ Libraries development tools
libboost-context1.54-dev - provides a sort of cooperative multitasking on a single thread
libboost-context1.54.0 - provides a sort of cooperative multitasking on a single thread
libboost-coroutine1.54-dev - provides a sort of cooperative multitasking on a single thread
libboost-exception1.54-dev - library to help write exceptions and handlers
libboost-graph-parallel1.54-dev - generic graph components and algorithms in C++
libboost-graph-parallel1.54.0 - generic graph components and algorithms in C++
libboost-graph1.54-dev - generic graph components and algorithms in C++
libboost-graph1.54.0 - generic graph components and algorithms in C++
libboost-locale1.54-dev - C++ facilities for localization
libboost-locale1.54.0 - C++ facilities for localization
libboost-log1.54-dev - C++ logging library
libboost-log1.54.0 - C++ logging library
libboost-math1.54-dev - Boost.Math Library development files
libboost-math1.54.0 - Boost.Math Library
libboost-mpi-python1.54-dev - C++ interface to the Message Passing Interface (MPI), Python Bindings
libboost-mpi-python1.54.0 - C++ interface to the Message Passing Interface (MPI), Python Bindings
libboost-mpi1.54-dev - C++ interface to the Message Passing Interface (MPI)
libboost-mpi1.54.0 - C++ interface to the Message Passing Interface (MPI)
libboost-random1.54-dev - Boost Random Number Library
libboost-random1.54.0 - Boost Random Number Library
libboost-signals1.54-dev - managed signals and slots library for C++
libboost-signals1.54.0 - managed signals and slots library for C++
libboost-timer1.54-dev - C++ wall clock and CPU process timers
libboost-timer1.54.0 - C++ wall clock and CPU process timers
libboost-wave1.54-dev - C99/C++ preprocessor library
libboost-wave1.54.0 - C99/C++ preprocessor library
libboost1.54-all-dev - Boost C++ Libraries development files (ALL)
```

libboost-dev를 설치하고 보니 이번에는 mysql 라이브러리가 잡히지 않아서 또 에러. 검색해보니 오늘 날짜 기준으로 mysql 관련 라이브러리는 다음과 같았다.

```
before_install.1
0.30s$ sudo apt-cache search mysql
bacula-common-mysql - network backup service - MySQL common files
bacula-common-mysql-dbg - network backup service - MySQL common files (debugging)
bacula-director-mysql - network backup service - MySQL storage for Director
bacula-director-mysql-dbg - network backup service - MySQL storage for Director (debugging)
bacula-sd-mysql - network backup service - MySQL SD tools
bacula-sd-mysql-dbg - network backup service - MySQL SD tools (debugging)
libapache2-mod-auth-mysql - Apache 2 module for MySQL authentication
libdatetime-format-mysql-perl - Parse and format MySQL dates and times
libdbd-mysql - MySQL database server driver for libdbi
libmysqlcppconn-dev - MySQL Connector for C++ (development files)
libmysqlcppconn7 - MySQL Connector for C++ (library)
python-mysqldb - Python interface to MySQL
python-mysqldb-dbg - Python interface to MySQL (debug extension)
aolserver4-nsmysql - AOLserver 4 module: module for accessing MySQL databases
asterisk-mysql - MySQL database protocol support for the Asterisk PBX
cl-sql-mysql - CLSQL database backend, MySQL
courier-authlib-mysql - MySQL support for the Courier authentication library
cvm-mysql - Credential Validation Modules (MySQL)
dbf2mysql - xBase <--> MySQL
dpm-copy-server-mysql - DPM copy server with MySQL database backend
dpm-name-server-mysql - DPM nameserver server with MySQL database backend
dpm-server-mysql - Disk Pool Manager (DPM) server with MySQL database backend
dpm-srm-server-mysql - DPM SRM server with MySQL database backend
dpsyco-mysql - Automate administration of access to mysql
dsyslog-module-mysql - advanced modular syslog daemon - MySQL support
emma - extendable MySQL managing assistant
falconpl-dbi-mysql - MySQL database abstraction layer for Falcon P.L
gambas3-gb-db-mysql - MySQL driver for the Gambas database component
gambas3-gb-mysql - Gambas MySQL component
gmysqlcc - graphical client for managing MySQL databases
gnokii-smsd-mysql - SMSD plugin for MySQL storage backend
gsql-mysql-engine - MySQL engine for GSQL
handlersocket-mysql-5.5 - HandlerSocket plugin for MySQL 5.5
kamailio-mysql-modules - MySQL database connectivity module for Kamailio
kexi-mysql-driver - MySQL support for kexi
letodms - document management system based on PHP and MySQL
lfc-server-mysql - LCG File Catalog (LFC) server with MySQL database backend
libapache2-mod-log-sql-mysql - Use SQL to store/write your Apache queries logs - MySQL interface
libaprutil1-dbd-mysql - Apache Portable Runtime Utility Library - MySQL Driver
libclass-dbi-mysql-perl - extensions to Class::DBI for MySQL
libcrypt-mysql-perl - Perl module to emulate the MySQL PASSWORD() function
libdbd-mysql-ruby - Transitional package for ruby-dbd-mysql
libdbd-mysql-ruby1.8 - Transitional package for ruby-dbd-mysql
libdbd-mysql-ruby1.9.1 - Transitional package for ruby-dbd-mysql
libdbix-fulltextsearch-perl - Indexing documents with MySQL as storage
libdspam7-drv-mysql - MySQL backend for DSPAM anti-spam filter
libgda-5.0-mysql - MySQL provider for libgda database abstraction library
libghc-hsql-mysql-dev - MySQL driver of the HSQL library for GHC
libghc-hsql-mysql-doc - API documentation of the hsql-mysql library for Haskell
libghc-hsql-mysql-prof - MySQL driver of the HSQL library for GHC; profiling libraries
libkaya-mysql-dev - MySQL binding for kaya
libmyodbc - the MySQL ODBC driver
libmysql++-dev - MySQL C++ library bindings (development)
libmysql++-doc - MySQL C++ library bindings (documentation and examples)
libmysql++3 - MySQL C++ library bindings (runtime)
libmysql-cil-dev - MySQL database connector for CLI
libmysql-diff-perl - module for comparing the table structure of two MySQL databases
libmysql-java - Java database (JDBC) driver for MySQL
libmysql-ocaml - OCaml bindings for MySql (runtime package)
libmysql-ocaml-dev - OCaml bindings for MySql (development package)
libmysql6.4-cil - MySQL database connector for CLI
libnss-mysql-bg - NSS module for using MySQL as a naming service
libopendbx1-mysql - MySQL backend for OpenDBX
libpam-mysql - PAM module allowing authentication from a MySQL server
librdf-storage-mysql - RDF library, MySQL backend
libtime-piece-mysql-perl - module adding MySQL-specific methods to Time::Piece
libwtdbomysql-dev - MySQL/MariaDB backend for Wt::Dbo [development]
libwtdbomysql35 - MySQL/MariaDB backend for Wt::Dbo [runtime]
lighttpd-mod-mysql-vhost - MySQL-based virtual host configuration for lighttpd
lua-dbi-mysql - DBI library for the Lua language, MySQL backend
lua-dbi-mysql-dbg - DBI library for the Lua language, MySQL backend debug symbols
lua-dbi-mysql-dev - DBI library for the Lua language, MySQL development files
lua-sql-mysql - luasql library for the Lua language
lua-sql-mysql-dev - luasql development files for the Lua language
mha4mysql-manager - Master High Availability Manager and Tools for MySQL, Manager Package
mha4mysql-node - Master High Availability Manager and Tools for MySQL, Node Package
monodoc-mysql-manual - compiled XML documentation for the MySql.Data library
mydumper - High-performance MySQL backup tool
mylvmbackup - quickly creating backups of MySQL server's data files
mysql-mmm-agent - Multi-Master Replication Manager for MySQL - agent daemon
mysql-mmm-common - Multi-Master Replication Manager for MySQL - common files
mysql-mmm-monitor - Multi-Master Replication Manager for MySQL - monitoring daemon
mysql-mmm-tools - Multi-Master Replication Manager for MySQL - tools
mysql-proxy - high availability, load balancing and query modification for mysql
mysql-utilities - collection of scripts for managing MySQL servers
mysql-workbench - MySQL Workbench - a visual database modeling, administration and queuing tool
mysql-workbench-data - MySQL Workbench -- architecture independent data
mysqltcl - interface to the MySQL database for the Tcl language
mysqltuner - high-performance MySQL tuning script
mysqmail - real-time logging system in MySQL
mysqmail-courier-logger - real-time logging system in MySQL - Courier traffic-logger
mysqmail-dovecot-logger - real-time logging system in MySQL - Dovecot traffic-logger
mysqmail-postfix-logger - real-time logging system in MySQL - Postfix traffic-logger
mysqmail-pure-ftpd-logger - real-time logging system in MySQL - Pure-FTPd traffic-logger
mytop - top like query monitor for MySQL
ndoutils-nagios3-mysql - This provides the NDOUtils for Nagios with MySQL support
node-mysql - MySQL client implementation for Node.js
nuauth-log-mysql - The authenticating firewall [MySQL log module]
oar-server-mysql - OAR batch scheduler MySQL server backend package
oar-user-mysql - OAR batch scheduler MySQL user backend package
opendnssec-dbg-mysql - Debug symbols for OpenDNSSEC (Enforcer with MySQL support)
opendnssec-enforcer-mysql - tool to prepare DNSSEC keys (mysql backend)
parser3-mysql - MySQL driver for Parser 3
pennmush-mysql - text-based multi-user virtual world server with MySQL support
percona-toolkit - Command-line tools for MySQL and system tasks
perdition-mysql - Library to allow perdition to access MySQL based popmaps
php-mdb2-driver-mysql - PHP PEAR module to provide a MySQL driver for MDB2
php5-mysqlnd-ms - MySQL replication and load balancing module for PHP
pike7.8-mysql - MySQL modules for Pike
postfix-cluebringer-mysql - metapackage for mysql support in postfix-cluebringer
postfix-gld - greylisting daemon for postfix, written in C, uses MySQL
puppet-module-puppetlabs-mysql - Puppet module for mysql
python-mysql.connector - pure Python implementation of MySQL Client/Server protocol
python3-mysql.connector - pure Python implementation of MySQL Client/Server protocol (Python3)
r-cran-rmysql - GNU R package providing a DBI-compliant interface to MySQL
ratbox-services-mysql - IRC services for use with ircd-ratbox with the mysql backend
redmine-mysql - metapackage providing MySQL dependencies for Redmine
root-plugin-sql-mysql - MySQL client plugin for ROOT
roundcube-mysql - metapackage providing MySQL dependencies for RoundCube
rt4-db-mysql - MySQL database backend for request-tracker4
ruby-dataobjects-mysql - MySQL adapter for ruby-dataobjects
ruby-dbd-mysql - Ruby/DBI MySQL driver
ruby-mysql - MySQL module for Ruby
ruby-mysql2 - simple, fast MySQL library for Ruby
spl-mysql - SPL Programming Language -- MySQL adapter
tarantool-mysql-plugin - Tarantool in-memory database - MySQL connector
tcl8.6-tdbc-mysql - Tcl Database Connectivity
tntdb-mysql4 - MySQL backend for tntdb database access library
ukolovnik - Simple todo manager using PHP and MySQL
ulogd-mysql - transitional dummy package for ulogd2-mysql
ulogd2-mysql - MySQL extension to ulogd
voms-mysql-plugin - VOMS server plugin for MySQL
voms-mysql-plugin-dbg - VOMS server plugin for MySQL - Debug Symbols
yate-mysql - MySQL support module for yate
zabbix-proxy-mysql - network monitoring solution - proxy (using MySQL)
zabbix-server-mysql - network monitoring solution - server (using MySQL)
exim4-daemon-heavy - Exim MTA (v4) daemon with extended features, including exiscan-acl
libdbd-mysql-perl - Perl5 database interface to the MySQL database
libmysqlclient-dev - MySQL database development files
libmysqlclient18 - MySQL database client library
libmysqld-dev - MySQL embedded database development files
libqt4-sql-mysql - Qt 4 MySQL database driver
libreoffice-base-drivers - Database connectvity drivers for LibreOffice
mysql-client - MySQL database client (metapackage depending on the latest version)
mysql-client-5.5 - MySQL database client binaries
mysql-client-core-5.5 - MySQL database core client binaries
mysql-common - MySQL database common files, e.g. /etc/mysql/my.cnf
mysql-server - MySQL database server (metapackage depending on the latest version)
mysql-server-5.5 - MySQL database server binaries and system database setup
mysql-server-core-5.5 - MySQL database server binaries
nova-api - OpenStack Compute - API frontend
nova-cert - OpenStack Compute - certificate management
nova-common - OpenStack Compute - common files
nova-compute - OpenStack Compute - compute node base
nova-compute-kvm - OpenStack Compute - compute node (KVM)
nova-compute-libvirt - OpenStack Compute - compute node libvirt support
nova-compute-lxc - OpenStack Compute - compute node (LXC)
nova-doc - OpenStack Compute - documentation
nova-network - OpenStack Compute - Network manager
nova-objectstore - OpenStack Compute - object store
nova-scheduler - OpenStack Compute - virtual machine scheduler
nova-volume - OpenStack Compute - storage
php5-mysql - MySQL module for php5
php5-sqlite - SQLite module for php5
postfix-mysql - MySQL map support for Postfix
python-nova - OpenStack Compute Python libraries
rsyslog - reliable system and kernel logging daemon
strongswan-plugin-mysql - strongSwan plugin for MySQL
strongswan-plugin-sql - strongSwan plugin for SQL configuration and credentials
akonadi-backend-mysql - MySQL storage backend for Akonadi
automysqlbackup - daily, weekly and monthly backup for your MySQL database
cacti - web interface for graphing of monitoring systems
collectd-core - statistics collection and monitoring daemon (core system)
dovecot-mysql - secure POP3/IMAP server - MySQL support
drizzle - Server binaries for Drizzle Database
drizzle-client - Client binaries for Drizzle Database
drizzle-dbg - Debugging symbols for Drizzle
drizzle-dev-doc - API Documentation for drizzle
drizzle-doc - Documentation for Drizzle
drizzle-plugin-auth-file - File-based authentication for Drizzle
drizzle-plugin-auth-http - HTTP authentication for Drizzle
drizzle-plugin-auth-ldap - LDAP authentication for Drizzle
drizzle-plugin-auth-pam - PAM authentication for Drizzle
drizzle-plugin-auth-schema - Schema authentication for Drizzle
drizzle-plugin-debug - Plugin that facilitates debugging Drizzle
drizzle-plugin-dev - Development files for Drizzle plugin development
drizzle-plugin-gearman-udf - Gearman User Defined Functions for Drizzle
drizzle-plugin-http-functions - HTTP Functions for Drizzle
drizzle-plugin-js - Javascript plugin for Drizzle
drizzle-plugin-json-server - JSON HTTP (NoSQL) interface for Drizzle
drizzle-plugin-logging-gearman - Gearman Logging for Drizzle
drizzle-plugin-logging-query - Query Logging for Drizzle
drizzle-plugin-perf-dictionary - Performance Dictionary for Drizzle
drizzle-plugin-query-log - Query logging for Drizzle
drizzle-plugin-rabbitmq - RabbitMQ Transaction Log for Drizzle
drizzle-plugin-regex-policy - Regex based authorization rules for Drizzle
drizzle-plugin-simple-user-policy - Simple User Policy for Drizzle
drizzle-plugin-slave - Replication Slave Plugin for Drizzle
freeradius-mysql - MySQL module for FreeRADIUS server
gnash-ext-mysql - GNU Shockwave Flash (SWF) player - MySQL extension
libdrizzle-dbg - library for the Drizzle and MySQL protocols, debug symbols
libdrizzle-dev - library for the Drizzle and MySQL protocols, development files
libdrizzle4 - library for the Drizzle and MySQL protocols
libdrizzledmessage-dev - Devel library containing serialized messages used with Drizzle
libdrizzledmessage0 - Library containing serialized messages used with Drizzle
libmariadbclient-dev - MariaDB database development files
libmariadbd-dev - MariaDB embedded database development files
libmysqld-pic - PIC version of MySQL embedded server development files
libphp-adodb - ADOdb database abstraction layer for PHP
libpocomysql9 - C++ Portable Components (POCO) MySQL library
libpocomysql9-dbg - C++ Portable Components (POCO) MySQL library (debug version)
libqt5sql5-mysql - Qt 5 MySQL database driver
libreoffice-mysql-connector - MariaDB/MySQL Connector extension for LibreOffice
libtango8 - TANGO distributed control system - shared library
libtango8-dbg - TANGO distributed control system - debug library
libtango8-dev - TANGO distributed control system - development library
libtango8-doc - TANGO distributed control system - documentation
mariadb-client-5.5 - MariaDB database client binaries
mariadb-client-core-5.5 - MariaDB database core client binaries
mariadb-server-5.5 - MariaDB database server binaries
mariadb-server-core-5.5 - MariaDB database core server files
mariadb-test-5.5 - MariaDB database regression test suite
mysql-client-5.6 - MySQL database client binaries
mysql-client-core-5.6 - MySQL database core client binaries
mysql-common-5.6 - MySQL 5.6 specific common files, e.g. /etc/mysql/conf.d/my-5.6.cnf
mysql-server-5.6 - MySQL database server binaries and system database setup
mysql-server-core-5.6 - MySQL database server binaries
mysql-source-5.5 - MySQL source
mysql-source-5.6 - MySQL source
mysql-testsuite - MySQL testsuite
mysql-testsuite-5.5 - MySQL testsuite
mysql-testsuite-5.6 - MySQL 5.6 testsuite
nova-ajax-console-proxy - OpenStack Compute - AJAX console proxy - transitional package
nova-api-ec2 - OpenStack Compute - EC2 API frontend
nova-baremetal - Openstack Compute - baremetal virt
nova-cells - Openstack Compute - cells
nova-compute-qemu - OpenStack Compute - compute node (QEmu)
nova-compute-vmware - OpenStack Compute - compute node (VMware)
nova-compute-xen - OpenStack Compute - compute node (Xen)
nova-conductor - OpenStack Compute - conductor service
nova-console - OpenStack Compute - Console
nova-consoleauth - OpenStack Compute - Console Authenticator
nova-novncproxy - OpenStack Compute - NoVNC proxy
nova-spiceproxy - OpenStack Compute - spice html5 proxy
nova-xvpvncproxy - OpenStack Compute - XVP VNC proxy
owncloud - empty package
pdns-backend-mysql - generic MySQL backend for PowerDNS
percona-xtradb-cluster-client-5.5 - Percona Server database client binaries
percona-xtradb-cluster-common-5.5 - Percona Server database common files
percona-xtradb-cluster-server-5.5 - Percona Server database server binaries
percona-xtradb-cluster-testsuite-5.5 - Percona Server database test suite
php5-mysqlnd - MySQL module for php5 (Native Driver)
phpmyadmin - MySQL web administration tool
pinba-engine-mysql-5.5 - realtime statistics server for PHP using MySQL as a read-only interface
proftpd-mod-mysql - Versatile, virtual-hosting FTP daemon - MySQL module
pure-ftpd-mysql - Secure and efficient FTP server with MySQL user authentication
rsyslog-mysql - MySQL output plugin for rsyslog
tango-db - TANGO distributed control system - database server
viewvc-query - utility to query CVS and Subversion commit database
mythtv-database - Personal video recorder application (database)
cl-pgloader - extract, transform and load data into PostgreSQL
cl-qmynd - MySQL Native Driver for Common Lisp
pgloader - extract, transform and load data into PostgreSQL
postgresql-10-mysql-fdw - Postgres 10 Foreign Data Wrapper for MySQL
postgresql-11-mysql-fdw - Postgres 11 Foreign Data Wrapper for MySQL
postgresql-9.3-mysql-fdw - Postgres 9.3 Foreign Data Wrapper for MySQL
postgresql-9.4-mysql-fdw - Postgres 9.4 Foreign Data Wrapper for MySQL
postgresql-9.5-mysql-fdw - Postgres 9.5 Foreign Data Wrapper for MySQL
postgresql-9.6-mysql-fdw - Postgres 9.6 Foreign Data Wrapper for MySQL
postgresql-mysql-fdw-doc - documentation for mysql-fdw
```

이 중에서 `libmysqlcppconn-dev`, `libmysqlcppconn7`, `libmysql++-dev`, `libmysql++3` 이렇게 4개의 패키지를 추가했다.

다시 빌드해보니... 안된다.

이번에는 tchar.h 에러. 이건 윈도우에만 있는 헤더... 문제는 MSSQL 관련 파일들이었다. 윈도우에서만 사용되도록 define 해주고 다시 커밋.

이 이외에도 무수히 많은 에러를 수정하며 지나가야했다. git-flow로 아예 브랜치를 별도로 만들어서 처리해나갔다.

  * `strncpy_s` 문제 : MS 전용이다. `strncpy` 로 함수 변경
  * `boost::filesystem::ofstream`을 `std::ofstream`으로 변경 : 함수 인자를 모두 수정해주어야 한다. 두 함수는 이름은 같지만 인자의 타입이 다르다.
  * `__super` 사용 금지 : MS 전용이다. gcc에서는 `__super` 사용 금지. `typedef`로 기본클래스를 지정해서 사용해야한다. (<https://goo.gl/hSeG97>)
  * 몇군데 rvalue reference에 대한 수정도 필요했다.

여기까지 수정했음에도 여전히 많은 오류에 부딪혔다.

결국 리눅스 빌드는 나중에 해보기로 하고 윈도우에서 빌드를 시작하는 것으로...
