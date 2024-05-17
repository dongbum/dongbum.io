---
title: Flask + IIS 로 윈도우에서 서비스하기
date: 2019-09-06T16:14:14+09:00
categories:
  - Python
tags:
  - Flask
  - IIS
  - python
  - 서버
  - 서비스
  - 연동
  - 윈도우
  - 파이썬
---
Flask로 만든 라이센스서버를 윈도우에 올려보기로 한다. 테스트할 환경은 다음과 같다.

  * OS : Microsoft Windows 10
  * Python : 3.6.8
  * Flask로 작성된 어플리케이션

---

### Flask와 IIS

원래는 Flask로 만들었으니 IIS에 붙이려고 했다. 그런데 그게 쉽지 않았다. IIS에 온갖 설정과 에러와 싸우며 조금씩 전진했지만 나는 지쳐갔고 정말 이게 최선인가...? 하는 생각이 들기 시작했다. 어차피 플라스크만으로도 괜찮은 웹서버 아니었던가? 굳이 IIS와 연동이 왜 필요한가? 에서 답을 찾지 못했다. 아파치나 nginx와 연동도 있었지만 이건 결국 플라스크로 만든 어플리케이션을 띄워놓은 상태에서 웹으로 들어오는 요청만 프록시 처리해주는 방식이라 더 의미 없는 것 같았다. 그래서 이걸 아예 윈도우서비스로 올릴 수 있으면 어떨까 싶었다.

---

### Web.config 파일 생성

<https://docs.microsoft.com/ko-kr/visualstudio/python/configure-web-apps-for-iis-windows?view=vs-2019>

웹페이지를 보며 설정한다. 간단하게 web.config 파일만 생성해주면 되었다.
```xml
<?xml version=1.0 encoding=utf-8?>
<configuration>
  <system.webServer>
    <handlers>
      <add   name=PythonHandler
            path=*
            verb=*
            modules=FastCgiModule
            scriptProcessor=C:\Python\Python36\python.exe|C:\Python\Python36\Lib\site-packages\wfastcgi.py
            resourceType=Unspecified
            requireAccess=Script ></add>
    </handlers>

  </system.webServer>

  <appSettings>
      <add key=PYTHONPATH value=D:\license_server></add>
      <!-- The handler here is specific to Bottle; see the next section. -->
      <add key=WSGI_HANDLER value=runserver.application></add>
      <add key=WSGI_LOG value=D:\logs\wfastcgi.log></add>
  </appSettings>
</configuration>
```
---

### wfastcgi-enable 실행

cmd를 관리자 권한으로 열고, 파이썬 프로그램이 있는 곳에 가서 **wfastcgi-enable** 을 실행해준다. IIS에서 사이트를 생성하고 접속해본다. 파일을 생성하고 실행하니 이상하게도 config 파일을 읽어오지 못하는 문제가 발생했다. 경로도 모두 맞는데 파일을 읽지 못하는 현상. 확인해봤더니 root 경로를 잘못 잡고 있었다. 경로 밑에 config 파일을 넣어주니 해결

---

### 참고자료

  * <http://mwultong.blogspot.com/2007/01/python-sysargv-command-line-argument.html>
  * <https://docs.microsoft.com/ko-kr/visualstudio/python/configure-web-apps-for-iis-windows?view=vs-2019>
