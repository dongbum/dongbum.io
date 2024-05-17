---
title: boost::asio::io_service::work 가 무엇인가
date: 2017-09-28T16:47:38+09:00
categories:
  - boost
tags:
  - asio
  - boost
---
예전에 만들어진 소스를 분석 중 `boost::asio::io_service::work` 가 나오길래 무엇인지 찾아보았다.

io_service 객체가 작업이 없으면 강제 종료되는 것을 막기 위한 장치라고 보면 될것 같다.

아주 잘 정리된 링크 : http://blog.naver.com/njh0602/221096093619
