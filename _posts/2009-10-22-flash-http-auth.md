---
title: 플래시에서 http-auth 방식 인증처리방법
date: 2009-10-22T02:25:56+09:00
categories:
  - Flash/Flex/ActionScript
tags:
  - Flash
  - http-auth
  - 인증
  - 플래시
---
플래시에서 http-auth 방식 인증처리방법
```actionscript
// http BASIC 인증 코드  
// http://www.abdulqabiz.com/blog/archives/flash_and_actionscript/http_authentica.php

var request:URLRequest = new URLRequest();

//call listsubs method of Bloglines
request.url = "";

var credentials:String = Base64.encode(email + ":" + password);
//create HTTP Auth request header
var authHeader:URLRequestHeader = new URLRequestHeader("Authorization","Basic " + credentials);
//add the header to request
request.requestHeaders.push(authHeader);
//make the request.
loader.load(request);
```
