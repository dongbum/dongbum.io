---
title: Google Breakpad 설치 (2)
date: 2018-12-24T16:34:44+09:00

categories:
  - C/C++
  - Programming
  - Python
tags:
  - Breakpad
  - C
  - gmock
  - Google
  - gtest
  - python
  - VisualStudio
---
gyp 파일을 다 실행하고 나면 솔루션 파일과 프로젝트 파일 등이 생겨난다. 비주얼스튜디오로 솔루션 파일을 열어보면 다음과 같이 여러개의 프로젝트가 솔루션 안에 들어있다.

![](/assets/images/google-breakpad-project-list.png)

맨 밑에 build_all 프로젝트를 빌드해보면...

84개의 오류와 28개의 경고로 빌드가 안된다. 뭐 어차피 기대도 안했다.

에러메시지를 보면

```cpp
#include "testing/gtest/include/gtest/gtest.h"
#include "testing/include/gmock/gmock.h"
```

이 부분에서부터 에러가 시작된다. gtest와 gmock이 필요하다고 한다. 이건 아마 외부라이브러리인 모양.

이 패키지들은 https://github.com/google/googletest 에서 받을 수 있다. 이 레포지토리를 git으로 클론한다. 다운로드 받으면 googlemock과 googletest다 들어있으며, 각 디렉토리에 msvc 라는 디렉토리가 있고 여기에 비주얼스튜디오로 열 수 있는 솔루션 파일과 프로젝트 파일들이 들어있다.

솔루션 파일들을 열어서 실행해보면 .lib 파일들이 생성된다. 이것들은 지금 당장은 필요 없다.

이제 이 파일들을 구조에 맞게 넣어줘야한다. 이걸 어떻게 해야하는지 몰라서 한참 찾았다.

https://github.com/Mendeley/breakpad

이 사람이 디렉토리 구조를 이미 만들어서 넣어놨길래 그대로 참고했다. (아마 이 사람은 브레이크패드에 맞도록 모든 라이브러리 파일을 다 넣어서 올려놓은듯하다. 이 소스를 받아다가 사용하면 잘될꺼 같기도하다.)

여기서부터 문제가 있는데, googlemock과 googletest 디렉토리를 구글브레이크패드 폴더에 복사해넣어줘야 한다. 그런데 내 경우에는, 회사컴퓨터에서 할 때와, 내 컴퓨터에서 했을 때 경로가 달랐다.

회사컴퓨터에서는... 구글 브레이크패드 디렉토리 중 src 디렉토리 밑에 testing 디렉토리를 만들고 거기에 googlemock 디렉토리 내용 전체를 복사해서 넣는다. 그리고 src/testing 디렉토리 밑에 gtest 디렉토리를 만들고 거기에는 googletest 디렉토리 내용 전체를 복사해서 넣는다.

집 컴퓨터에서는... 구글 브레이크 패드 디렉토리 중 src 디렉토리 밑에 testing 디렉토리를 만들고 거기에 googlemock과 googletest 디렉토리 두개를 그대로 복사해넣었다.

이제 구글 브레이크패드 솔루션 파일을 열고 build_all 프로젝트를 빌드해보면...

안된다.

오류가 난다. 오류내용은

```
C4091 'typedef ': 변수를 선언하지 않으면 '' 왼쪽은 무시됩니다.
C2220 경고가 오류로 처리되어 생성된 'object' 파일이 없습니다.
```

해당 코드는 다음과 같이 되어있다.

```cpp
typedef enum {
    hdBase = 0, // root directory for dbghelp
    hdSym,      // where symbols are stored
    hdSrc,      // where source is stored
    hdMax       // end marker
};
```

enum인데 이름이 정의되지 않은 상태로 typedef가 되었기 때문에 나는 에러이다.

이걸 검색해보니 다음과 같은 내용을 찾았다.

https://stackoverflow.com/questions/913344/how-can-i-remove-the-vs-warning-c4091-typedef-ignored-on-left-of-spreadsh

typedef를 그냥 삭제해버리랜다. 삭제하려고 봤더니...

이 오류가 나는건, `C:\Program Files (x86)\Windows Kits\8.1\Include\um\DbgHelp.h` 파일이었다. C++ 라이브러리를 수정할 수는 없는 노릇... **'프로젝트 속성'의 '구성 속성' -> 'C/C++' -> '고급'을 찾아가 '특정 경고 사용 안 함' 에 4091을 추가했다.** 이제 빌드가 된다.

같은 에러가 나는 프로젝트를 찾아서 **전부 위처럼 4091 에러에 대한 경고무시를 설정**해줘야한다.

이렇게 또 build_all 을 해봤더니 또 오류가 난다.

프로젝트 중 unittests 에 있는 processor_bits 프로젝트가 계속 에러가 나는 것이었다. 경고를 전부 오류 처리해버리기에 빌드가 되지 않았다. 이것도 4091 에러처럼 **4267, 4366** 을 경고무시에 추가한다.

이제 다시 build_all 프로젝트를 빌드해본다.

또 오류가 난다.

unittest 에 있는 client_tests 프로젝트에서 에러가 났다. 마찬가지로 경고를 오류로 처리해서 나는 에러. **4389, 4312, 4267** 을 경고무시로 넣어준다.

다시 build_all 프로젝트를 빌드해본다.

![](/assets/images/google-breakpad-build-success.png)

드디어 아무 에러 없이 성공.

이제 빌드된 구글브레이크패드를 이용해서 실제 사용하는 방법을 찾아봐야겠다.
