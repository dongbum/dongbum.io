---
title: Visual Assist X Snippet 기능을 이용해 Doxygen 주석 자동 입력하기
date: 2014-11-19T11:00:17+09:00
categories:
  - Programming
tags:
  - Doxygen
  - Snippet
  - Visual Assist X
  - Visual Studio
  - 비주얼 스튜디오
---
Visual Studio 2012에 Visual Assist를 설치하여 사용하면서 가장 쓸모있는 기능중 하나는 Snippet 기능이다.

Snippet 기능을 이용하여 특정 함수에 주석을 달 때 Doxygen 형식의 주석을 자동으로 삽입하게 할 수 있다. Snippet을 설정하려면 Visual Studio의 VASSISTX 메뉴에서 다음처럼 들어간다.

![](/assets/images/doxygen-description-menu.png)

'Edit VA Snippets...' 메뉴에 들어간다. VA Snippet Editor 화면이 열리면 왼쪽 트리메뉴 중 C++를 선택하고 Refactor Document Method를 선택한다.

![](/assets/images/doxygen-description-va-snippet.png)

오른쪽 하단의 주석 내용들이 함수 설명으로 들어갈 부분들이다.

비주얼 스튜디오의 '도구' -> '옵션'을 눌러 옵션창을 연다.

![](/assets/images/doxygen-description-option.png)

옵션 창에서 '환경' -> '키보드'를 선택한 다음, '다음 문자열을 포함하는 명령 표시'에 RefactorDocumentMethod를 입력하면 Visual Assist X의 Refactor Document Method 가 검색된다.

'바로 가기 키 누르기' 밑의 하얀 텍스트 박스를 클릭한 다음, 자기가 원하는 단축키를 입력하고 '할당' 버튼을 클릭한다. 내 경우에는 Shift + Alt + M으로 설정해놓는다.

이제 비주얼 스튜디오로 와서 함수 위에 커서를 놓고 방금 설정한 단축키를 누르면 다음과 같이 자동으로 주석이 입력된다.

![](/assets/images/doxygen-description-add-description.png)

주석의 모양이라던가 입력 방식을 바꾸고 싶다면 다시 Edit Snippet 창에 가서 수정해주면 바로 반영된다. 지금 이 입력 양식은 디폴트 양식으로 입력하였으나 나중의 문서화를 위해서는 Doxygen 형식을 참조하여 입력하는 것이 좋다고 생각한다.

스니펫에 입력 가능한 매크로에 대해서는 <http://ekessy.tistory.com/26>을 참조하면 될 것 같다.
