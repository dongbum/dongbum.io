---
title: 과유불급의 보안정책
date: 2021-11-10T23:09:36+09:00
categories:
  - DevStory
tags:
  - Security
---

### 강력한 비밀번호 정책

제가 겪었던 어떤 회사는 비밀번호 정책이 매우 강력했습니다.

당연하게도, 특수문자, 영어 대/소문자, 숫자를 섞어서 비밀번호를 만들어야 했습니다. 그리고 이 비밀번호는 대략 한달 정도 지나면 다른 비밀번호로 교체를 요구했습니다. 비밀번호 교체를 할 때에는 이전에 바꿨던 3개의 비밀번호는 사용할 수 없었습니다. 만약 비밀번호 입력시 다섯번이 틀리면 계정이 잠겨버리고 복잡한 비밀번호 초기화 작업을 해야했습니다.

이런 복잡한 비밀번호 규칙 때문에 대부분의 사람들은 비밀번호를 크롬의 비밀번호 저장 기능을 통해 저장해서 사용했고 그냥 대충대충 저장하고 까먹는 일이 다반사였구요.

하지만 크롬의 비밀번호 저장기능이 만능인 것도 아닌게, 사이트의 도메인이 엇비슷하고 리다이렉트 기능이 난무하다보니 이 비밀번호 기능도 사이트를 헷갈려하기 시작합니다. A 사이트인데 B 사이트의 비밀번호를 자동대입해주는 것이지요.

이렇게 비밀번호를 다섯번 틀려버리면 또 비밀번호 초기화 정책에 의하여 비밀번호를 다시 설정하라고 합니다. 그리고 저 복잡한 비밀번호 규칙을 지켜야하며, 직전 3개의 비밀번호는 또 쓸 수 없었습니다.

이렇게 복잡한 비밀번호 규칙을 통하여 과연 보안이 강력해졌을까?

### 하지만 사람들은

결론적으로, 팀원들은 공용계정을 만들기로 했습니다. (물론 보안담당자는 공용계정을 허용하지 않습니다만 어차피 안 걸리면 그만입니다.)

모두가 쓰는 공용계정을 하나 만들고 비밀번호를 모두가 공유했습니다. 그리고 비밀번호는 한사람(보통 막내팀원이 맡겠죠.)이 전담해서 관리하고 모두가 비밀번호를 돌려씁니다.

보안 따위는 이제 없습니다. 누가 그 계정을 사용했는지 실제로 알 수도 없습니다.

뭐 문제가 터지면... 그때는 어떻게 되려나요? 하지만 다행히도 그런 일은 일어나진 않았습니다. 하하.

### 과유불급

너무 과도한 보안정책은 사람들의 불편함을 초래합니다.

매일매일 사용해야하는 업무용 사이트를 한달에 한번씩 비밀번호를 바꾸게 하고 이전 비밀번호까지 계속 쓰지 못하게 하니 사람들이 계속 복잡한 비밀번호를 생성하여 외울 수는 없는 노릇입니다. 비밀번호를 잃어버리면 또 복잡한 절차가 기다리고 있죠.

적당한 비밀번호 정책을 만들어주고 IP 대역을 이용한 보안정책 등을 이용했어야 하는데 막무가내식으로 복잡하고 강력한 비밀번호 정책만 강요했습니다. (이 모든 사태가 보안인증 때문이라는 말도 있습니다.) 사람들은 그렇게 멍청하지 않습니다. 필요 이상의 복잡하고 불편한 것이 있다면 어떻게든 그것을 빠져나가고 부수려하지 거기에 순응하지 않기 마련입니다.

보안관리자는 저런 말도 안되는 정책을 받아들일 것으로 정말 기대했을까요?
