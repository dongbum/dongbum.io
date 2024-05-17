---
title: C#에서 Byte[]와 String간 변환하기
date: 2013-01-23T11:51:10+09:00
categories:
  - 'C#'
tags:
  - byte
  - C
  - string
---
서버에서 받은 데이터를 byte[]에 문자열을 저장하고 Message.Show()로 보여주려고 했더니 System.Byte[]만 계속 찍혔다.

뭐가 문제인가 네이버에서 찾아봤더니 좋은 글 발견.

String을 byte[]로 변환하려면,

```csharp
byte[] ba = System.Text.Encoding.Default.GetBytes(str);
```

byte[]를 String으로 변환하려면,  

```csharp
String str = System.Text.Encoding.Default.GetString(ba);
```

출처 : <http://blog.naver.com/hursh1225?Redirect=Log&logNo=40120911491>
