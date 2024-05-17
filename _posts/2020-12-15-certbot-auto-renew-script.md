---
title: certbot 자동 갱신 스크립트
date: 2021-03-02 11:29:01
categories:
  - Linux
tags:
  - Linux
  - SSL
---

AWS Lightsail 에서 certbot 을 이용하여 https를 사용하고 있다.
Let's encrypt 인증서는 90일마다 갱신해줘야해서 귀찮았는데 아래와 같은 스크립트를 찾았다. 실행해보니 일단 잘 작동한다.

crontab에 걸어놓고 인증서가 잘 갱신되는지 확인해봐야겠다.

```shell
#!⁄bin⁄bash
# sudo test
if [[ $(⁄usr⁄bin⁄id -u) -ne 0 ]]; then
  echo "Not running as root"
  exit
fi
# test valid date left.
TESTDATE=`certbot certificates | grep VALID`
ATD=($TESTDATE)
LEFTDATE=`expr ${ATD[5]}`
echo "date left : ${LEFTDATE}"
if [ ${LEFTDATE} -lt 30 ]; then
  echo "Left date less than a month, OK, proceeding update !"
  # service apache2 stop
  certbot renew
  # service apache2 start
  service apache2 restart # or nginx restart
else
  echo "No need to update, yet"
fi
```

이 스크립트의 출처는 [자유로운 그날을 위해](https://rageworx.tistory.com/1926) 이다.

##### 2021년 3월 2일 추가

3개월간 작동시켜보니 위 스크립트에 문제점이 있었다.

실제로 갱신처리가 되지 않는 경우가 있었는데, certbot renew 를 하기 전에 웹서버를 stop 하는 것이 문제였다. certbot이 renew 작업을 할 때에는 웹서버의 특정 URL (.well-known)에 http로 접속해서 도메인의 주인임을 확인하는 절차가 있는데 웹서버가 꺼져있으니 이 작업이 실패되어 갱신 작SSL 갱신 작업이 실패하게 되었다.

certbot renew는 웹서버가 실행된 상태에서도 작동이 가능하므로, certbot renew 명령 이후 웹서버를 restart 시키도록 해야 정상적으로 작동한다.
