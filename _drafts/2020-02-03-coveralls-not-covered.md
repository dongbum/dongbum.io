---
title: coveralls not covered
date: 2020-02-03 11:15:38
categories:
  - C/C++
tags:
  - C++
---

coveralls에서 다음과 같은 문제를 볼 수 있었다.

![](/assets/images/coveralls-not-covered.png)

소스코드는 모두 100% 커버되었는데 프로젝트 자체는 0%인 상황.

구글에 검색해보니 다음과 같은 자료를 찾을 수 있었다.

<https://stackoverflow.com/questions/37373398/why-does-coveralls-show-0-coverage-when-every-source-file-is-100-covered>

gcov와 g++의 버전이 맞지 않아서 그렇다는 답변이었다.

### 참고자료
* <https://stackoverflow.com/questions/37373398/why-does-coveralls-show-0-coverage-when-every-source-file-is-100-covered>
