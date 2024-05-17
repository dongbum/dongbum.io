---
title: boost 1.70.0 버전 업그레이드시 참고사항
date: 2019-05-08T18:17:10+09:00
categories:
  - boost
  - C/C++
tags:
  - boost
  - C
---
boost를 1.60.0 버전을 계속 사용하다가 1.70.0으로 업그레이드하니 변경된 점이 많았다.

<https://zepeh.tistory.com/498> 를 참고한다.

**ASIO에서 io_service가 io_context 로 변경되었다.** 하지만 1.70.0 버전을 적용하면 io_service도 그대로 사용할 수 있긴 하다. cpp 파일과 h 파일도 그대로 들어있다. 비주얼스튜디오 인텔리센스에서도 io_context 가 바로 인식되지는 않는데, 이런 경우 전처리기 설정에서 **BOOST_ASIO_NO_DEPRECATED** 를 추가해주면 io_service 가 비활성화되고 io_context만 사용할 수 있게 된다.

io_context 로 코드를 바꾸고 나면 충돌사항들이 나오게 된다.

#### from_string 변경

`from_string()` 함수가 삭제되었다.

아래 코드를

```cpp
boost::asio::ip::address_v4 address = boost::asio::ip::address_v4::from_string(server_ip, error_code);
```

아래처럼 변경한다.

```cpp
boost::asio::ip::address_v4 address = boost::asio::ip::make_address_v4(server_ip, error_code);
```

이 문제도 해결.

#### strand.wrap() 변경

strand의 `wrap()` 함수도 변경되었다.

다음의 코드를

```cpp
boost::asio::async_write(socket_,
    boost::asio::buffer(send_queue_.front().first, send_queue_.front().second),
    strand_.wrap(
        boost::bind(
            &BasicSocket::OnSendHandler,
            shared_from_this(),
            boost::asio::placeholders::error,
            boost::asio::placeholders::bytes_transferred
        )
    )
);
```

아래처럼 변경한다.

```cpp
boost::asio::async_write(socket_,
    boost::asio::buffer(send_queue_.front().first, send_queue_.front().second),
    boost::asio::bind_executor(strand_, boost::bind(
        &BasicSocket::OnSendHandler,
        shared_from_this(),
        boost::asio::placeholders::error,
        boost::asio::placeholders::bytes_transferred
    )
    )
);
```

이 문제도 해결.
