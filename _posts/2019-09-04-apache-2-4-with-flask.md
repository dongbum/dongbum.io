---
title: apache 2.4 + flask 설정방법
date: 2019-09-04T18:55:15+09:00
categories:
  - Linux
  - Python
---
아파치에서 Flask로 만들어진 어플리케이션을 작동하기 위한 방법을 정리해둔다. 에러메시지는

```console
    [Mon Jul 03 19:42:27.334280 2017] [:error] [pid 14570] [client 59.10.233.103:63211] Traceback (most recent call last):
    [Mon Jul 03 19:42:27.334301 2017] [:error] [pid 14570] [client 59.10.233.103:63211] File /home/viper9/python_test/VeloWeight.wsgi, line 7, in <module>
    [Mon Jul 03 19:42:27.334349 2017] [:error] [pid 14570] [client 59.10.233.103:63211] from hello import app as application
    [Mon Jul 03 19:42:27.334364 2017] [:error] [pid 14570] [client 59.10.233.103:63211] File /home/viper9/python_test/hello.py, line 1, in <module>
    [Mon Jul 03 19:42:27.334392 2017] [:error] [pid 14570] [client 59.10.233.103:63211] from flask import Flask
    [Mon Jul 03 19:42:27.334409 2017] [:error] [pid 14570] [client 59.10.233.103:63211] ImportError: No module named flask
    [Mon Jul 03 19:42:27.432767 2017] [:error] [pid 14538] [client 59.10.233.103:63210] mod_wsgi (pid=14538): Target WSGI script '/home/viper9/python_test/VeloWeight.wsgi' cannot be loaded as Python module., referer: http://python.dongbumkim.com/
    [Mon Jul 03 19:42:27.432821 2017] [:error] [pid 14538] [client 59.10.233.103:63210] mod_wsgi (pid=14538): Exception occurred processing WSGI script '/home/viper9/python_test/VeloWeight.wsgi'., referer: http://python.dongbumkim.com/
    [Mon Jul 03 19:42:27.432848 2017] [:error] [pid 14538] [client 59.10.233.103:63210] Traceback (most recent call last):, referer: http://python.dongbumkim.com/
    [Mon Jul 03 19:42:27.432870 2017] [:error] [pid 14538] [client 59.10.233.103:63210] File /home/viper9/python_test/VeloWeight.wsgi, line 7, in <module>, referer: http://python.dongbumkim.com/
    [Mon Jul 03 19:42:27.432904 2017] [:error] [pid 14538] [client 59.10.233.103:63210] from hello import app as application, referer: http://python.dongbumkim.com/
    [Mon Jul 03 19:42:27.432914 2017] [:error] [pid 14538] [client 59.10.233.103:63210] File /home/viper9/python_test/hello.py, line 1, in <module>, referer: http://python.dongbumkim.com/
    [Mon Jul 03 19:42:27.432928 2017] [:error] [pid 14538] [client 59.10.233.103:63210] from flask import Flask, referer: http://python.dongbumkim.com/
    [Mon Jul 03 19:42:27.432963 2017] [:error] [pid 14538] [client 59.10.233.103:63210] ImportError: No module named flask, referer: http://python.dongbumkim.com/
```

<http://flask.pocoo.org/docs/0.12/deploying/mod_wsgi/> 를 읽어보니 다음과 같이 써있었다.

> Working with Virtual Environments Virtual environments have the advantage that they never install the required dependencies system wide so you have a better control over what is used where. If you want to use a virtual environment with mod_wsgi you have to modify your .wsgi file slightly. Add the following lines to the top of your .wsgi file: activate_this = \'/path/to/env/bin/activate_this.py\' execfile(activate_this, dict(**file**=activate_this)) For Python 3 add the following lines to the top of your .wsgi file: activate_this = \'/path/to/env/bin/activate_this.py\' with open(activate_this) as file_: exec(file_.read(), dict(**file**=activate_this))

```
execfile(activate_this, dict(**file**=activate_this))
```

를 삭제하고

```
with open(activate_this) as file_: exec(file_.read(), dict(**file**=activate_this))
```

를 추가했다. 하지만 다시 실행해도 같은 에러... wsgi 파일을 다음과 같이 수정했다.

```python
import os
import sys
import site

site.addsitedir('/home/viper9/python_test/venv/lib64/python3.4/site-packages')

sys.path.insert(0, '/home/viper9/python_test')

activate_this = '/home/viper9/python_test/venv/bin/activate_this.py'
with open(activate_this) as file_:
exec(file_.read(), dict(file=activate_this))

from hello import app as application
```

MySQL 에러가 난다면 다음의 패키지를 설치한다.

```
yum install MySQL-python
```

해결했더니 그 다음 에러...

```
UnicodeDecodeError: 'ascii' codec can't decode byte 0xae in position 8: ordinal not in range(128)
```

<https://libsora.so/posts/python-hangul/> 를 찾아보니 인코딩의 문제라고 한다. 문서를 보고 해결. .wsgi 에 설정을 추가했다.

```python
reload(sys)
sys.setdefaultencoding('utf-8')
```

그런데... 그래도 해결이 안된다... 아 힘들다. 다시 이런저런 정보를 찾아헤메이다 로그를 다시 읽어보니 DB를 사용하는 곳에서만 에러가 났다. DB에서 데이터 가져올 때 문제가 있구나.. 라는 생각이 들어 DB 설정을 확인했더니 utf-8로 이미 다 설정되어 있었다. 코드도 UTF-8이고 DB도 UTF-8인데 데이터 가져오는데 문제가 있다면 커넥션 자체에서 캐릭터셋을 지정해주지 않아서 그런 것 같았다. 다시 검색을 해보니 다음 문서를 발견

<https://stackoverflow.com/questions/10819192/sqlalchemy-result-for-utf-8-column-is-of-type-str-why>

create_engine() 함수를 쓸 때

```python
python
create_engine('mysql+mysqldb:///mydb?charset=utf8')
```

처럼 사용하라는 것이었다. 이 설정을 추가해주니 드디어 페이지가 정확하게 떴다. 아이고 힘들다...
