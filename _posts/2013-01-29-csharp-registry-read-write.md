---
title: 'C#에서 레지스트리 읽고 쓰는 방법'
date: 2013-01-29T14:11:05+09:00
categories:
  - 'C#'
tags:
  - C
  - 레지스트리
---
Registry 클래스를 이용한다.

일단 소스 상단에 `using Microsoft.Win32`를 선언해준다.

```csharp
using Microsoft.Win32;

RegistryKey reg = Registry.LocalMachine.CreateSubKey("Software").CreateSubKey("RegistryKeyTest");

// SetValue()를 통해 값을 설정하고 GetValue()를 통해 값을 읽어올 수 있다.

reg.SetValue("Text", "글을 입력하겠소"); // 값을 저장한다.
reg.GetValue("Text", "없음")     // text라는 이름을 가진 값을 가져온다.
// 이때 값이 없다면 "없음" 이라고 값을 얻어온다.
reg.GetValue("Text") // text라는 이름을 가진 값을 가져온다.

// 레지스트리에 값을 삭제할 때는 DeleteSubkey()를 쓴다.

reg.DeleteSubKey("Text", false); // 값을 삭제한다.
Registry.LocalMachine.DeleteSubKey("Software\RegistryKeyTest"); // 레지스트리키를 삭제한다.
```

출처 : <http://happyguy81.blog.me/10148698440>

**2013년 5월 13일 추가.**

이 기능을 사용하려면 프로그램을 관리자 권한으로 실행해야 정상작동한다. 관리자 권한으로 실행하지 않았을시에는 오류가 발생할 수 있다. 때문에 내 경우에는 힘들여만들어놓은 이 기능을 안 쓰고 파일을 이용하도록 변경해 나가는 중.
