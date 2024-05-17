---
title: double 타입 사용시 주의할 점
date: 2019-07-03T18:59:46+09:00
categories:
  - C/C++
  - Note of the wrong answers
---
오늘 삽질했던 코드. 평소에는 double을 잘 안 쓰다보니 헷갈렸다.

문제가 되었던 코드.

```cpp
for (auto iter = range.first; iter != range.second; iter++)
{
    int reward_id = (*iter).second.reward_id;
    int funcsrl = (*iter).second.funcsrl;
    int reward_type = (*iter).second.reward_type;
    std::string reward_itemcode = (*iter).second.itemcode;
    double rate = (*iter).second.rate / 100;	// DB에는 만분률로 되어있으므로 백분율로 환산해야함.

    lottery_info.Add(std::make_tuple(reward_id, funcsrl, reward_type, reward_itemcode), rate);

#ifdef _DEBUG
    ServerStateLog(g_DebugTracerServer, "RewardID:[%d] FuncSRL:[%d] Rate:[%f]", reward_id, funcsrl, rate);
#endif
}
```

확률 계산처리를 해야하는데 DB에는 데이터가 만분율 기준(ex : 9.5% = 950)으로 들어가 있고 확률 계산처리 함수에는 백분율로 넣어줘야했다. 그래서 데이터를 가져오면서 나누기 100을 했던 것인데...

이터레이터 안의 (*iter).second.rate 값은 int 값. 950을 100으로 나누어 그 결과인 9.5를 double 형으로 얻으려는 것이었다.

확률 계산에서 문제가 계속 일어나 찾아보니 저 값이 9.5가 아닌 9.0이 들어가고 있었다.

int 와 int 를 나누어서 double 에 넣는다고 소수점 처리가 되지 않는다는 것을 깜빡했다. / 100 을 / 100.0f 로 수정하는 것으로 쉽게 해결.

* 간단히 참고 : <https://ycswarm.tistory.com/130>
