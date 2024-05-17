---
title: JSP를 응용하여 계정용량 파악해보기
date: 2012-03-27T03:35:31+09:00
categories:
  - Java/JSP
tags:
  - Java/JSP
  - JSP
  - linux
  - 계정용량
  - 리눅스
  - 서버
  - 자바
---
내 서버에서는 무료로 웹호스팅 서비스를 운영 중인데 사용자지원페이지를 만들어보았다.

사용자지원페이지에는 현재 계정용량 파악하기라는 메뉴를 넣기로 했는데 정작 사용자 계정용량 파악하는 방법을 찾을 수 없었다는 것. 그래서 예전에 PHP로 만들었던 기억과 구글에서 찾은 몇가지 정보를 더하여 계정용량 검색기능을 만들어보았다.

일단 리눅스서버에서 쓰는 계정용량 검색 명령어는 `du -shm /home/계정ID`의 형태이다. 이 시스템명령어를 수행하여 결과값을 가져오면 된다는게 기본 원리.

```java
<%@page import=java.io.InputStreamReader%>
<%@page import=java.io.BufferedReader%>
<%
String command = du -shm /home/ + id;

//계정 총용량 구하기
String info_space = ;
Process p = null;
BufferedReader br = null;

try {
    p = Runtime.getRuntime().exec(command);
    br = new BufferedReader(new InputStreamReader(p.getInputStream()));
    String line = null;
    while ((line = br.readLine()) != null) {
        line = line.split(/)[0].trim();
        info_space = 약  + line +  MByte;
    }
} catch (Exception e) {
    info_space = 계정 총용량을 확인할 수 없습니다.;
}
%>
```

자바에서는 시스템명령을 수행할 때 Runtime 클래스를 사용하면 된다. 이 클래스를 사용해서 돌아온 결과값을 while을 이용하여 읽어들이면(여기에서 `BufferedReader`와 `InputStreamReader` 클래스가 필요하다.) 명령을 수행한 결과를 알 수 있다.

내가 뽑고 싶은건 계정사용 공간이었는데 리눅스의 `du -shm` 명령어는 잡다한 글귀가 붙어있기 때문에 이것을 split와 trim 함수를 이용해서 필요한 부분만 잘라주었다. 이 부분은 아마 정규표현식이나 아니면 다른 어떠한 방법으로 더 간단히 할 수 있을 것 같지만 일단 내가 아는 명령어는 이정도이므로. du 명령어의 옵션으로 준 -h는 사람이 읽기 쉬운 형태로 보여준다. 예를 들면, 기가바이트 단위의 데이터는 G로 메가바이트는 M로 표시해준다. 일관성이 없어지기 때문에 웹페이지에서도 이것을 표시할 때 일관성이 없어지게 되므로 추가해서 -m 옵션을 주어서 무조건 Mega Byte 단위로 처리하게 만들었다. 이 방법을 이용하면 생각보다 쉽게 계정용량을 MByte 단위로 환산하여 출력할 수 있게 된다.

한가지 주의할 점은 생각보다 계정용량이 정확하게는 표시되지 않는다. 또한 자바의 Runtime 클래스는 어찌된 영문인지 접근권한에 상관 없이 실행될 수 있는 것 같다. 악의적인 사용자가 rm 등의 명령어를 사용할 시 크게 문제가 될 수 있을 수 있으니 실행할 명령어를 파라미터로 받아서 수행한다거나 하는 경우는 절대 없어야 할 것 같다.
