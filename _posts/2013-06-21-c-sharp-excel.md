---
title: 'C#에서 엑셀 문서 저장하기'
date: 2013-06-21T11:04:13+09:00
categories:
  - 'C#'
tags:
  - C
  - ClosedXML
  - Excel
  - OpenXML
  - 엑셀
---
![](/assets/images/ClosedXML_small2.png)

C#으로 사내용으로 쓸 프로그램을 만들다가 엑셀 파일로 저장해야할 일이 생겼다.

인터넷을 찾아헤메다가 결국은 내가 사용하게 된 방법에 대해 정리한다.

찾아본 끝에 내린 결로은 OpenXML 기술을 이용하는 방식으로 택했다. .xlsx라는 가장 최신의 엑셀 포맷이고 MS오피스 뿐만 아니라 오픈오피스에서도 잘 지원한다. OpenXML이 무엇인가에 대해서는 구체적으로 나도 잘 모르겠다. 혹시 관심 있는 사람들은 위키 같은 곳을 찾아봄이 좋을듯하다. 난 엑셀 파일 읽고 쓰기가 필요한 것이지 엑셀 포맷 그 자체에는 별로 관심도 없거니와 알 필요성도 적어서(물론 알면 좋겠지만) 일단은 OpenXML에 대해 잠깐만 웹서핑을 해본 후 사용하기로 결정했다. OpenXML에 대해 더 알고 싶다면 다음의 페이지를 방문해보자.

  * <http://en.wikipedia.org/wiki/Office_Open_XML>
  * <http://ko.wikipedia.org/wiki/OpenXML>
  * <http://msdn.microsoft.com/ko-kr/office/bb265236.aspx>

OpenXML을 사용하기 위해서 ClosedXML 이라는 라이브러리를 이용할 것이고 <http://closedxml.codeplex.com> 를 참조하면 된다.

내 환경은 Visual Studio 2012 이며 C# Winform .NET 3.5 였는데 이 기능을 이용하기 위해서는 .NET 4.0 으로 프로젝트 설정을 변경해야했다. 4.0 아래 버전에서는 호환이 안되기 때문에 오류가 난다.

다운로드 받은 후 압축을 풀어보면 몇개의 DLL파일과 XML 파일 등으로 이루어져 있으며 C#을 사용했던 사람이라면 별로 어려움 없이 사용할 수 있으리라 본다.

예제코드는 <http://closedxml.codeplex.com/wikipage?title=Showcase&referringTitle=Home> 에 설명되어 있고 이것을 참조하면서 만들면 별 무리가 없다.

엑셀 파일을 작성하는데 필요한 기초적인 기능은 충분히 제공된다. 결과적으로는 제대로 선택한 것 같고 별 어려움 없이 엑셀 문서를 작성할 수 있었다.

하나 주의할 사항이라면,

엑셀 파일로 작성할 내용이 많아지면 속도가 꽤나 느려질 수 있다. 특히 다수의 셀에 색상을 입힌다거나 Style 속성을 바꾸는 작업 등은 속도를 굉장히 느려지게 할 수 있으니 주의해야한다. 다수의 셀에 특정 Style을 적용하고 싶다면 for 루프를 돌면서 각 셀마다 Style을 지정하지 말고

```csharp
ws.Range("B" + index.ToString() + ":D" + index.ToString()).Style.Font.Bold = true;
ws.Range("B" + index.ToString() + ":D" + index.ToString()).Style.Font.FontColor = XLColor.White;
ws.Range("B" + index.ToString() + ":D" + index.ToString()).Style.Fill.BackgroundColor = XLColor.Black;
```

와 같은 코드를 사용해서 한번에 잡아주는 것이 속도 향상에 도움이 된다.

예제페이지처럼 RangeTable을 잡고 한번에 적용해주면 더 속도향상이 되리라 생각하지만 이렇게 하기에는 너무 귀찮아져서 일단은 난 행단위로 처리했다.

(이 ClosedXML 라이브러리를 이용했다면 프로그램 배포시 ClosedXML.DLL 파일도 같이 배포해야하니 이것 역시 주의하자.))

개발이 모두 끝나고서 알게된 것이지만 ClosedXML은 .NET 4.0용과 .NET 3.5용이 있었다. 이런 젠쟝... (<http://closedxml.codeplex.com/releases/view/96561>)
