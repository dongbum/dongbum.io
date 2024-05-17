---
title: 랜덤확수의 확률 변경과 분포에 대한 문제
date: 2012-09-22T18:55:06+09:00
categories:
  - Note of the wrong answers
  - Programming
tags:
  - 랜덤
  - 면접
  - 프로그래밍
  - 확률
---
이 질문 역시 내가 면접에서 질문 받았던 문제이다. 틀렸던 문제를 다시 한번 살펴보고 공부하는 차원에서 오답노트에 적어둔다. (Q는 면접관님, A는 내가 답변한 것이다. 존칭은 생략한다.)

* Q. 랜덤함수에서 0과 1이 나올 확률이 같은가?
* A. 같다. 내가 알기로는 0~1까지의 소수가 나오는 것으로 알고 있다. (여기서 대답을 잘못했다. 플래시의 랜덤함수와 C에서의 랜덤함수를 헷갈렸던 것이다.)
* Q. 정말? 그렇다면 0~10까지 나오게 하려면 어떻게 해야하는가?
* A. r이 그 랜덤함수값이라고 한다면 (int)(r * 10)으로 구할 수 있다.
* Q. 그렇게 한 경우에는 0~10까지 모든 수가 나올 확률은 같은가?
* A. 같다고 알고 있다.
* Q. 만약 1~5까지의 어떤 수를 뽑는 랜덤함수가 있다면 확률을 바꾸고 싶다. 1,5는 가장 적게 나오도록, 2,4는 그다음으로 많이 나오도록 3은 제일 많이 나오도록 결과적으로는 확률분포가 삼각형 모양이 되게 하고 싶다. 어떻게 하면 되겠는가?

여기까지가 질문이었는데 확률을 바꾸라는 말에 제대로 대답을 못했다.

일단 첫번째는 랜덤함수는 0~1까지의 소수가 나오는 것이 아니었다. 그리고 랜덤함수의 확률 분포는 생각보다 그렇게 고르지 않다고 한다. (이에 대한 자료는 <http://agebreak.tistory.com/49> 와 <http://ljh131.tistory.com/131> 에 있다.) 내 생각에는 랜덤을 반복할 수록 점점 확률이 고르게 분포하지 않을까 싶다.

위 면접관분이 질문했던 내용을 찾아보니 정확하게는 균등분포를 정규분포로 바꾸는 문제였다. 이에 대한 자료는 <http://tadis.tistory.com/14> 를 참조하면 될것 같다. 그런데 이 코드를 봐도 sqrt는 알겠지만 log 등을 사용하는 구간에서 잘 이해가 안된다. 더군다나 이 코드는 내가 확률을 조정하기보다는 정규분포에 맞추어 바꿔주는 역할을 하는 코드인 것 같다.

다음은 간단히 1부터 5까지 랜덤한 수를 100000개 뽑아보는 코드다.

```cpp
#include <iostream>
#include <time.h>

using namespace std;
#define MAX_TEST 100000
int main()
 {
 int count[5] = {0};
 int percent[5] = {10, 20, 40, 20, 10};
srand((unsigned)time(NULL));
 for (int i = 0; i < MAX_TEST; i++)
 {
 int r = rand() % 5 + 1;
 count[r - 1]++;
 }
float total = 0.0;
 for (int i = 0; i < 5; i++)
 {
 cout << i + 1 << "의 갯수 :: " << count[i] << " - " << count[i] / (float)(MAX_TEST / 100) << "%" << endl;
 total += (count[i] / (float)(MAX_TEST / 100));
 }
return 0;
 }
```

여기서 알 수 있는건 분포가 정확하게 20%씩은 아니라는 점이었다.

이 코드를 1부터 5까지 랜덤한 수를 정해진 비율에 맞도록 100000개 뽑아내는 코드로 변경한 것이다.

```cpp
#include <iostream>
#include <time.h>
using namespace std;
#define MAX_TEST 100000
int main()
 {
 int count[5] = {0};
 int percent[5] = {10, 20, 40, 20, 10};
srand((unsigned)time(NULL));
 for (int i = 0; i < MAX_TEST; i++)
 {
 int r = rand() % 100;
if (r < 10)
 {
 count[0]++;
 }
 else if (r < 30)
 {
 count[1]++;
 }
 else if (r < 70)
 {
 count[2]++;
 }
 else if (r < 90)
 {
 count[3]++;
 }
 else if (r < 100)
 {
 count[4]++;
 }
 }
 for (int i = 0; i < 5; i++)
 {
 cout << i + 1 << "의 갯수 :: " << count[i] << " - " << count[i] / (float)(MAX_TEST / 100) << "%" << endl;
 }
return 0;
 }
```

단순히는 이렇게 하면 정해진 비율에 유사하게 값이 나온다.

추가로 난수발생에 대한 좋은 글들은 <http://oops.kldp.org/node/103420> , <http://agebreak.tistory.com/49> , <http://terzeron.net/wp/?p=1006> 에서 찾을 수 있었다.

비율이 유사하게가 아니라 정확한 비율대로 나오려면 배열에 미리 가능한 수를 넣어놓고 랜덤하게 위치를 변경시키는 방법 밖에는 아직 생각나지 않는다. 조금 더 생각하고 공부해봐야 할 문제인 것 같다.
