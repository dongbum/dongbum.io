---
title: CentOS 6.2에서 Cacti 설치시 graphs.php 페이지 오류 해결방법
date: 2012-04-02T02:23:32+09:00
categories:
  - Linux
tags:
  - cacti
  - graph-viz
  - graphs.php
---
Cacti를 설치했는데 Graph Management 페이지가 아예 빈페이지로 나왔다.

이런 경우는 PHP 오류인 경우인데 내 서버 경우에는 PHP 오류시 오류메시지 출력이 꺼져있으므로 빈페이지가 그냥 출력되는 경우이다.

이 문제를 해결하기 위해 또 구글링을 해봤다.

다행히 Cacti 포럼에 다음의 글이 올라와있었다.

<http://forums.cacti.net/viewtopic.php?f=2&t=46455>

문제는 graphviz라는 프로그램이라는 것이었고 이것을 삭제하였더니 잘 작동하더라는 것이다. 내 서버에도 확인해봤더니 역시나 저 프로그램이 설치되어 있었다. 무엇에 쓰는 프로그램인지도 잘 모르는채로 삭제할 수 없어서 다시 위 글타래를 잘 읽어보니 다음의 링크에서 해결방법을 찾을 수 있었다.

<http://bugs.cacti.net/view.php?id=2161>

아마 Cacti의 이슈트래킹 시스템인 것 같은데 다행히도 이 문제를 해결해서 패치파일로 제공하고 있었다. 이 페이지에 링크 걸려있는 cacti_graph.diff 파일을 cacti 디렉토리에 다운로드 받은 후 다음 명령을 적용해준다.

```console
patch -p0 < cacti_graph.diff
```

몇줄의 안내메시지가 나온 후 패치가 끝난다. 다시 Graph Management 페이지를 접속하면 잘 보이게 된다. 이 버그가 신고된게 올해 2월 1일이었고 2월 22일에 패치가 적용되었다. 불과 한달 전에 이 패치가 개발되었으니 얼마나 다행인지 모르겠구나. 흠흠..
