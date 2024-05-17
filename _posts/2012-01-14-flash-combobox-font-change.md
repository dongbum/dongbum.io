---
title: 플래시에서 콤보박스 폰트 변경
date: 2012-01-14T20:09:07+09:00
categories:
  - Flash/Flex/ActionScript
tags:
  - ComboBox
  - Flash
  - Font
  - Style
  - 폰트
  - 플래시
---
플래시에서 ComboBox 컴포넌트를 사용했는데 클라이언트측의 요구로 콤보박스의 폰트를 변경해달라는 부탁이 왔다.

플래시 레퍼런스에는 이러한 내용들에 대해 기술된 것이 없어서 고민하다가 구글을 찾아보니 이미 외국에서 나와 똑같은 고민을 했던 사람이 있었다.

<http://www.designscripting.com/2011/06/as3-combobox-font-embedding-problem-flash-cs5/>

```actionscript
var arial:Font = new ArialFont();

var myFormatBlack:TextFormat = new TextFormat();
myFormatBlack.font = arial.fontName;
myFormatBlack.size = 18;
myFormatBlack.color = 0x000000;

myComboBox.textField.setStyle("embedFonts", true);
myComboBox.textField.setStyle("textFormat", myFormatBlack);
myComboBox.dropdown.setRendererStyle("embedFonts", true);
myComboBox.dropdown.setRendererStyle("textFormat", myFormatBlack);
myComboBox.setStyle("embedFonts", true);
myComboBox.setStyle("textFormat", myFormatBlack);
myComboBox.prompt = "Select State";
myComboBox.width = 248;
myComboBox.height = 25;
myComboBox.x = 100
myComboBox.y = 100
myComboBox.setStyle("textPadding", 1);
```

위의 코드를 참조하여 콤보박스의 폰트를 바꿀 수 있었다. 중요한 것은 라이브러리에서 폰트를 미리 등록해놔야 한다.

사용할 폰트를 임베디드 한 후 라이브러리에서 폰트를 선택한 후 properties를 선택해서 액션스크립트에서 사용할 수 있도록 체크하고 이름을 지정한다. 내 경우에는 윤고딕 같은 폰트는 알아보기 쉽게 'Yoon'이라는 이름으로 사용했다. 위의 코드에서는 폰트명을 'ArialFont'라고 지정하고 있다. 이것이 되지 않으면 저 코드도 아무 소용이 없다.

폰트를 임베디드했으면 콤보박스의 각 부위에 setStyle과 setRendererStyle 메서드를 이용하여 적용한다.

이 두가지만 잘 이해했다면 콤보박스에 폰트를 적용하는 것은 어렵지 않을 것 같다.
