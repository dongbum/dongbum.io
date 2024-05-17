---
title: C++/Python 환경에서 Coveralls + Travis-CI로 코드커버리지 테스트
date: 2019-10-08T15:13:10+09:00
categories:
  - C/C++
  - GIT
  - Python
tags:
  - C
  - coveralls
  - python
  - Travis-CI
  - 코드커버리지
  - 파이썬
---
코드 커버리지에 대한 설명들. <https://blog.outsider.ne.kr/954>

코드 커버리지를 테스트하기 위한 방법들을 정리해본다.

---

### C++에서 코드커버리지 테스트

C++이나 gcc를 사용하고 있다면 g++ 컴파일 옵션에 --coverage 를 넣는다. 만약, cmake를 사용한다면, 다음과 같은 코드로 코드커버리지 테스트와 coveralls까지 연동 가능하다

<https://github.com/dongbum/DServer/blob/master/.travis.yml>

```yaml
language: cpp

os:
- linux

compiler:
- gcc

dist: bionic

before_install:
- pip install --user cpp-coveralls
- sudo add-apt-repository ppa:mhier/libboost-latest -y
- sudo apt update
- sudo apt-get install libjemalloc-dev libjemalloc1 libjemalloc-dev libjsoncpp-dev libjsoncpp1 libmysqlcppconn-dev libmysql++-dev libboost1.70-dev

env:
  global:
  - MAKEFLAGS=-j 2

script:
- cmake CMakeLists.txt
- make
- ./GameServer

after_success:
- coveralls --exclude external_library --exclude make_gcc --exclude vs_solution --exclude CMakeFiles --gcov-options '\-lp';
```

cmake로는 따로 옵션이 필요 없었다.

---

### Python에서 코드커버리지 테스트

[https://docs.coveralls.io/python](python-coveralls) 와 coveralls-python 두가지를 추천하고 있다. python-coveralls로 설정하던 중 다음과 같은 에러를 만났다.

![](/assets/images/python-coveralls-error.png)

다음과 같이 간단한 Traivs-CI 스크립트로 커버리지 테스트를 성공했다.

```yaml
language: python
python:
  - 3.6
install:
  - pip install -r requirements.txt
  - pip install coverage
  - pip install coveralls
script:
  - python3 update.py
  - coverage run update.py
after_success:
  - coveralls
```
다만 coverage 명령은 run을 같이 해야되기 때문에 프로그램 실행되어야만 한다. 그래서 이 방법은 바로 실행되는 파이썬 프로그램에 한해서만 가능하다.

---

### Python Flask에서 코드커버리지 테스트

Flask 프레임워크를 사용하면 웹서버를 바로 실행시키는 형태로 프로그램이 작동하는지라 위의 coverage run 명령어로 테스트를 할 수가 없다. 이 명령으로 테스트를 하면 웹서버가 실행되고 프로세스가 종료되지 않기 때문에 travis의 빌드도 영원히 계속된다. 물론 이 상태로 10분 정도가 지나면 travis가 빌드실패로 처리해버린다. 따라서 Flask를 쓸때는 travis-ci에서 실제 프로그램을 실행시키는 형태가 아니라 코드커버리지 테스트만 수행해야한다. 이렇게 작동하기 위해서는 nose라는 또다른 코드커버리지 툴이 필요했다. travis-ci 스크립트는 다음처럼 같이 작성했다.

```yaml
language: python
services:
  - mysql
python:
  - 3.6
before_install:
  - mysql -e 'CREATE DATABASE IF NOT EXISTS test;'
install:
  - pip install -r requirements.txt
  - pip install coverage
  - pip install coveralls
  - pip install nose
  - pip install nose-exclude
script:
  - nosetests --with-coverage --cover-erase --cover-package=board
after_success:
  - coveralls
```

nosetests는 프로그램을 실제로 수행하지 않고도 코드커버리지만 테스트할 수 있기에 Flask로 작성된 프로그램들은 nose를 이용해야할 것 같다. 번외로, Flask를 사용하면서도 위에 있는 coverage run 명령을 이용할 수도 있는데, 웹서버를 실행시킨 상태로 특정 경로(예를 들면, /exit 같은...)을 호출하면 웹서버가 스스로 종료되도록 하는 방법이다. 하지만 이 방법은 그야말로 테스트를 위해 경로를 만들고 테스트만을 위한 종료코드까지 만들어야하는 관계로 nose를 쓰는 방법이 더 나은 것 같다.

---

### 참고자료

  * <https://code-maven.com/coverall-with-python-minimal-setup>
  * <https://kez1994.tistory.com/378>
