---
title: Redis Cluster
date: 2019-10-29 20:48:38
categories:
  - Database
  - Redis
tags:
  - Redis
  - Cluster
  - 데이터베이스
---

Redis Cluster에 구축방법을 작성한다. 하나의 인스턴스로 레디스는 작동하여 보았지만 클러스터 모드는 처음 접한다.

---

## Redis의 작동모드

#### Standalone
가장 기본적인 형태. 레디스 하나를 실행하여 사용한다.

#### Master-Slave
읽고/쓰기를 담당하는 마스터와 읽기만을 담당하는 슬레이브가 있어서 서로 상호보완한다.

#### Master-Slave-Sentinel
센티널 서버는 일종의 레디스 전용 모니터링 서버. 마스터-슬레이브와 같지만 센티널서버가 있어서 마스터 서버가 다운되면 슬레이브 서버를 마스터 서버로 변경해준다. 센티널 서버 역시 단일로 구성할 수도 있고 여러대를 구성할 수도 있다.

#### Cluster
센티널 서버가 없는 대신, 마스터-슬레이브 서버군을 여러대로 하여 상호간에 샤드-레플리케이션을 이루고 고가용성을 구현한다.

마스터-슬레이브 서버군 3개가 필요하여 최소 총 6대의 서버가 클러스터를 구성한다.

---

## Redis Cluster 구성

---

#### conf 파일 작성

redis-5001.conf 파일에서

```conf
port 5001
cluster-enabled yes
cluster-config-file /home/centos/redis/redis-cluster/node-5001.conf
cluster-node-timeout 3000
dir "/home/centos/redis/redis-cluster/node-5001/data"
appendonly yes
```

형식으로 클러스터를 활성화한다.

마스터, 슬레이브 상관 없이 모두 다 같은 형식의 설정으로 사용한다.
설정 내용 중에 있는 node-5001.conf 파일은 존재하지 않지만 이 설정대로 레디스를 실행하고 나면 파일이 자동으로 생성되게 된다.

---

#### 노드 등록

설치 완료 후 redis-cli 를 이용해 제일 첫번째 노드에 다른 노드들을 등록시킨다.

```console
redis-cli -h 127.0.0.1 -p 5001 cluster meet 127.0.0.1 5002
(...중략...)
redis-cli -h 127.0.0.1 -p 5001 cluster meet 127.0.0.1 5006
```

마지막으로 cluster nodes 명령으로 등록된 노드들을 확인한다.

```console
[centos@ip-172-26-7-248 redis]$ redis-cli -h 127.0.0.1 -p 5001 cluster nodes
94fefe320dbbdc2e4cc9ed97ad6971931e8a9dfb 127.0.0.1:5003@15003 master - 0 1572367588563 2 connected
14b561a8f9d3e2dbb17e6a0e1cdaad0b9e4cb959 127.0.0.1:5005@15005 master - 0 1572367587561 4 connected
2c5ca8b0dec7afda81d1805a7aa6aa3c8c4de81c 127.0.0.1:5002@15002 master - 0 1572367588061 1 connected
167aaddf91854a71fe46fda8694be96810c2fdbb 127.0.0.1:5004@15004 master - 0 1572367588061 3 connected
44cc12a6792b43cd5fc964f0693dbac5ffaaecc3 127.0.0.1:5006@15006 master - 0 1572367588563 5 connected
e0c5da84dec4deb5578406b8e1d2dd1ff71cd012 127.0.0.1:5001@15001 myself,master - 0 1572367588000 0 connected
[centos@ip-172-26-7-248 redis]$
```

---

#### Slave 노드 등록

redis-cli 명령으로 슬레이브 노드를 마스터 노드에 등록한다.

```console
redis-cli -h [슬레이브노드IP] -p [슬레이브노드포트] cluster replicate [붙어야할 마스터 노드ID]
```

마스터 노드ID는 nodes 명령으로 볼 수 있다.

```console
[centos@ip-172-26-7-248 redis]$ redis-cli -h 127.0.0.1 -p 5004 cluster replicate e0c5da84dec4deb5578406b8e1d2dd1ff71cd012
17947:S 30 Oct 01:56:19.575 * Before turning into a slave, using my master parameters to synthesize a cached master: I may be able to synchronize with the new master with just a partial transfer.
OK
[centos@ip-172-26-7-248 redis]$ 17947:S 30 Oct 01:56:20.273 * Connecting to MASTER 127.0.0.1:5001
17947:S 30 Oct 01:56:20.273 * MASTER <-> SLAVE sync started
17947:S 30 Oct 01:56:20.273 * Non blocking connect for SYNC fired the event.
17947:S 30 Oct 01:56:20.273 * Master replied to PING, replication can continue...
17947:S 30 Oct 01:56:20.273 * Trying a partial resynchronization (request c34e3c02004a4c8e06fcb47bc77ea45f138c7512:1).
17863:M 30 Oct 01:56:20.273 * Slave 127.0.0.1:5004 asks for synchronization
17863:M 30 Oct 01:56:20.273 * Partial resynchronization not accepted: Replication ID mismatch (Slave asked for 'c34e3c02004a4c8e06fcb47bc77ea45f138c7512', my replication IDs are 'c7d75434e77299659d54bb0826287f9f97ac9f17' and '0000000000000000000000000000000000000000')
17863:M 30 Oct 01:56:20.273 * Starting BGSAVE for SYNC with target: disk
17863:M 30 Oct 01:56:20.274 * Background saving started by pid 22762
17947:S 30 Oct 01:56:20.274 * Full resync from master: 5ed80233221922dec0393de2100c4d48903d1a70:0
17947:S 30 Oct 01:56:20.274 * Discarding previously cached master state.
22762:C 30 Oct 01:56:20.278 * DB saved on disk
22762:C 30 Oct 01:56:20.278 * RDB: 0 MB of memory used by copy-on-write
17863:M 30 Oct 01:56:20.368 * Background saving terminated with success
17863:M 30 Oct 01:56:20.368 * Synchronization with slave 127.0.0.1:5004 succeeded
17947:S 30 Oct 01:56:20.368 * MASTER <-> SLAVE sync: receiving 176 bytes from master
17947:S 30 Oct 01:56:20.368 * MASTER <-> SLAVE sync: Flushing old data
17947:S 30 Oct 01:56:20.368 * MASTER <-> SLAVE sync: Loading DB in memory
17947:S 30 Oct 01:56:20.368 * MASTER <-> SLAVE sync: Finished with success
17947:S 30 Oct 01:56:20.369 * Background append only file rewriting started by pid 22763
17947:S 30 Oct 01:56:20.393 * AOF rewrite child asks to stop sending diffs.
22763:C 30 Oct 01:56:20.393 * Parent agreed to stop sending diffs. Finalizing AOF...
22763:C 30 Oct 01:56:20.393 * Concatenating 0.00 MB of AOF diff received from parent.
22763:C 30 Oct 01:56:20.393 * SYNC append only file rewrite performed
22763:C 30 Oct 01:56:20.394 * AOF rewrite: 0 MB of memory used by copy-on-write
17947:S 30 Oct 01:56:20.473 * Background AOF rewrite terminated with success
17947:S 30 Oct 01:56:20.473 * Residual parent diff successfully flushed to the rewritten AOF (0.00 MB)
17947:S 30 Oct 01:56:20.473 * Background AOF rewrite finished successfully

[centos@ip-172-26-7-248 redis]$
```

최종적으로 다음과 같은 형태가 된다.

```console
[centos@ip-172-26-7-248 redis]$ redis-cli -h 127.0.0.1 -p 5001 cluster nodes
94fefe320dbbdc2e4cc9ed97ad6971931e8a9dfb 127.0.0.1:5003@15003 master - 0 1572368364897 2 connected
14b561a8f9d3e2dbb17e6a0e1cdaad0b9e4cb959 127.0.0.1:5005@15005 slave 2c5ca8b0dec7afda81d1805a7aa6aa3c8c4de81c 0 1572368365599 4 connected
2c5ca8b0dec7afda81d1805a7aa6aa3c8c4de81c 127.0.0.1:5002@15002 master - 0 1572368365599 1 connected
167aaddf91854a71fe46fda8694be96810c2fdbb 127.0.0.1:5004@15004 slave e0c5da84dec4deb5578406b8e1d2dd1ff71cd012 0 1572368365599 3 connected
44cc12a6792b43cd5fc964f0693dbac5ffaaecc3 127.0.0.1:5006@15006 slave 94fefe320dbbdc2e4cc9ed97ad6971931e8a9dfb 0 1572368364597 5 connected
e0c5da84dec4deb5578406b8e1d2dd1ff71cd012 127.0.0.1:5001@15001 myself,master - 0 1572368364000 0 connected
[centos@ip-172-26-7-248 redis]$
```
