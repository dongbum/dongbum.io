---
title: 플래시에서 입력값을 숫자로만 받는 방법~
date: 2008-05-07T13:42:00+09:00
categories:
  - Flash/Flex/ActionScript
---

특정한 입력박스에서 제한된 글자만 입력 받을 수 있게하는 방법

```actionscript
stop();  
form_name.restrict = "^0-9"; //숫자만 입력불가  
form_age.restrict = "0-9"; //숫자만 입력가능  
form_sex.restrict = "ㄱ-힣"; //한글만 입력가능  
form_phone.restrict = "0-9"; //숫자만 입력가능  
form_phone.maxChars = "9"; //최대 입력 문자수  
bt.onRelease = function(){  
  onEnterFrame = function() {  
    if(form_email.text.indexOf("@")<0 || form_email.text.indexOf(".")<0) {  
      trace("이메일 형식이 맞지 않습니다");  
    }else{ //"@", "."이 없을때  
      write=new LoadVars();  
      write.name = user_name;  
      write.age = user_age;  
      write.sex = user_sex;  
      write.email = user_email;  
      write.phone = user_phone;  
      write.sendAndLoad("test.html",write,"POST");  
    }  
    this.onEnterFrame = null;  
  }  
}  
```
