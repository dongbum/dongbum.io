---
title: 'CString to wchar_t *'
date: 2012-08-15T20:00:59+09:00
categories:
  - C/C++
tags:
  - C
  - CString
  - MFC
  - wchar_t
---
메신저를 만들어보고 있는데 서버는 C++와 MFC로 클라이언트는 WPF와 C#으로 만들어보고 있다.

패킷을 주고 받을 때 유니코드화 해야해서 `wchar_t*` 형태로 패킷을 보내게 만들었는데 이러다보니 `CString`형을 `wchar_t*` 형으로 변환해야될 경우가 많았다.

인터넷에 찾아보니 방법은 여러가지였지만 이 방법이 가장 짧고 편한 것 같다.

```cpp
wchar_t *szFileName;
CString strFileName = _T("J:\RefPrograms\WordCreationTest200\REPORT_test.docx");
USES_CONVERSION;
szFileName= A2W(strFileName.GetBuffer());
```

위 방법은 <http://blog.naver.com/ibb31?Redirect=Log&logNo=60100539710>의 내용이다.
