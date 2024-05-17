---
title: Visual Studio 에서 cURL 설치
date: 2019-08-12T17:38:16+09:00
categories:
  - C/C++
tags:
  - C
  - cURL
  - VisualStudio
---
윈도우에서 웹페이지와 통신할 때 WinHTTP 모듈을 쓰고 있었는데 이 모듈은 윈도우 전용이므로 리눅스에서는 사용이 불가능하다. cURL 모듈을 사용할 수 있는지 테스트. vcpkg로 cURL을 간단히 설치할 수 있었다. 그러나.... !

![](/assets/images/curl-error.png)

도저히 해결이 불가능했다. 구글과 스택오버플로우를 열심히 뒤졌으나 빌드플랫폼이 맞지 않아서 그럴거라는 의견뿐. x86 라이브러리와 x64 라이브러리를 모두 받아서 테스트 해봤으나 실패했다. vcpkg에 대한 환상이 좀 깨졌다. 직접 받아서 빌드해보기로 했다. cURL은 git 에서 소스 관리를 하고 있다. cURL을 클론한다.

`git clone https://github.com/curl/curl.git curl.git`

다운 받은 폴더의 projects/Windows 폴더에 가면 Visual Studio 버전별로 쭉 있다. 난 2015이므로 VC12 선택. curl-all.sln 솔루션 파일을 더블클릭하여 VS2015로 열어본다.

![](/assets/images/curl-project-load-error.png)

왜인지 모르겠는데 프로젝트 로드가 안된다. 프로젝트 파일이 없나 해당 위치에 가서 살펴봤는데.... 진짜로 프로젝트 파일이 없다! 어떻게 해야되나 고민하던 중에 project 폴더를 보니 여러개의 배치파일이 보였다. 일단 checksrc.bat 파일을 실행했다. 한참 아무메시지가 없더니만 프로그램이 종료되었다. 별 문제가 없으면 별 메시지를 출력하지 않나보다. (bat 파일 내용을 읽어보진 않았다.) 이제 왠지 프로젝트를 생성해줄 것 같은 generate.bat 파일을 실행했다. 예상대로...

```console
    D:\Library\curl.git\projects>checksrc.bat

    D:\Library\curl.git\projects>generate.bat

    Generating prerequisite files
    * D:\Library\curl.git\Makefile
    * D:\Library\curl.git\src\tool_hugehelp.c

    Generating VC6 project files
    * D:\Library\curl.git\projects\Windows\VC6\src\curl.dsp
    * D:\Library\curl.git\projects\Windows\VC6\lib\libcurl.dsp

    (...중략...)

    Generating VC12 project files
    * D:\Library\curl.git\projects\Windows\VC12\src\curl.vcxproj
    * D:\Library\curl.git\projects\Windows\VC12\lib\libcurl.vcxproj

    (...중략...)

    D:\Library\curl.git\projects>
```

예상대로 프로젝트 파일을 생성해주었다. 이제 다시 아까 VC12 폴더로 가서 솔루션 파일을 여니까 프로젝트가 보인다! 솔루션 빌드를 눌렀더니.... ...안된다...

```console
    1>d:\library\curl.git\lib\ssh.h(28): fatal error C1083: 포함 파일을 열 수 없습니다. 'libssh2.h': No such file or directory
    1>..\..\..\..\lib\md5.c(88): fatal error C1083: 포함 파일을 열 수 없습니다. 'openssl/md5.h': No such file or directory
    1>..\..\..\..\lib\md4.c(31): fatal error C1083: 포함 파일을 열 수 없습니다. 'openssl/opensslconf.h': No such file or directory
```

메시지가 쭉 박혀있었다. 다시 project 폴더에 가서 왠지 ssl 문제를 해결해줄 것 같은 build-openssl.bat 파일을 실행시키니 다음과 같은 안내가 나왔다.

```console
    D:\Library\curl.git\projects>build-openssl.bat

    Usage: build-openssl <compiler> [platform] [configuration] [directory] [-VSpath] [VSpath] [-perlpath] [perlpath]

    Compiler:

    vc6       - Use Visual Studio 6
    vc7       - Use Visual Studio .NET
    vc7.1     - Use Visual Studio .NET 2003
    vc8       - Use Visual Studio 2005
    vc9       - Use Visual Studio 2008
    vc10      - Use Visual Studio 2010
    vc11      - Use Visual Studio 2012
    vc12      - Use Visual Studio 2013
    vc14      - Use Visual Studio 2015
    vc14.1    - Use Visual Studio 2017

    Platform:

    x86       - Perform a 32-bit build
    x64       - Perform a 64-bit build

    Configuration:

    debug     - Perform a debug build
    release   - Perform a release build

    Other:

    directory - Specifies the OpenSSL source directory

    -VSpath - Specify the custom VS path if Visual Studio is installed at other location
              then C:/<ProgramFiles>/Microsoft Visual Studio[version]
              For e.g. -VSpath C:\apps\MVS14

    -perlpath - Specify the custom perl root path if perl is not located at C:\Perl and it is a
                portable copy of perl and not installed on the win system
                For e.g. -perlpath D:\strawberry-perl-5.24.3.1-64bit-portable

    D:\Library\curl.git\projects>
```

어렵구나... OpenSSL을 같이 연동해줘야하는것 같은데 정녕 이 방법 밖에 없는 것인지 다시 생각해봤다. 다시 vcpkg를 쓰는 방법을 찾아본다. 많은 글들을 참고로 했다. ( <http://bugsfixed.blogspot.com/2017/05/vcpkg.html> )

**이것저것 해보던 중. 갑자기 프로그램이 정상작동 되었다. -_-;;**

**설정은 프로젝트 설정에서 추가 포함 디렉토리로 include와 추가 라이브러리 디렉토리로 lib 디렉토리만 설정했고 다른 설정은 모두 삭제했다.**

프로그램을 빌드해봤더니 바이너리 파일과 함께 libcurl.dll, zlib1.dll 파일이 자동으로 생성되었다. 이제 cURL을 테스트 해볼 차례.

---

### 참고자료

  * cURL 사용하여 웹페이지 긁어오기 : <http://sobusted.cf/221485638633> 이 예제 중 fstream 관련하여 약간 문제가 있어 수정해서 사용했다.
  * libcurl 메뉴얼 : <https://curl.haxx.se/libcurl/c/>
  * 테스트 해봤던 프로젝트 : <https://github.com/dongbum/CURLTest>
