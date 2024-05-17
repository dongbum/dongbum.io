---
title: Python에서 MySQL 연동
date: 2012-11-25T14:28:26+09:00
categories:
  - Python
tags:
  - MySQL
  - python
---
일단 서버에 파이썬을 설치한다. 내 경우에는 CentOS 6.3에 파이썬은 2.6 버전이다. yum으로 설치할 수 있는 가장 최신 버전.

mysql.com에서 파이썬 커넥터를 찾는다. 현재는 <http://www.mysql.com/downloads/connector/python/#downloads> 에서 찾을 수 있다. 리눅스에서 설치할 것이므로 Platform Independent 버전을 다운로드 받아야한다.

다운로드 받은 파일을 압축을 폴면 디렉토리 하나와 그 안에 여러개의 파일들이 들어가 있다.

폴더 안에 들어가서

```
python setup.py build
```

명령을 준다. 파일들이 복사된다는 메시지들이 쭉 뜬다.

```
python setup.py install
```

명령을 내린다. 역시 파일들이 복사된다는 메시지들이 쭉 뜬다.

python 명령을 치고 파이썬 쉘에서 import MySQLdb 를 입력해본다. 별 문제가 없다면 설치 완료.
