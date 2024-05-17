---
title: 메모리복사시 침범 문제
date: 2017-11-29T09:19:47+09:00
categories:
  - C/C++
---
boost asio를 이용하여 코드를 작성 중에 다음과 같은 부분이 있었다.

클래스의 멤버변수로 네트워크 처리에 사용할 버퍼를 만들고 변수를 생성한 코드다.

```cpp
class BasicSocket : public std::enable_shared_from_this<BasicSocket>
{
// 중략
protected:
    std::shared_ptr<Socket> socket_ptr_;

    Socket socket_;

    char packet_buffer_[RECV_BUFFER_SIZE * 2];
    int32_t remain_size_;

    char recv_buffer_[RECV_BUFFER_SIZE];
    char send_buffer_[SEND_BUFFER_SIZE];
};
```

이후 이 변수들을 사용해서 다음과 같은 코드를 작성했다.

```cpp
std::cout << "OnReceive 1 remain_size_:" << remain_size_ << " - bytes_transferred:" << bytes_transferred << std::endl;

// 패킷을 담아둘 버퍼에 수신버퍼의 내용을 복사한다.
memcpy(&packet_buffer_[remain_size_], recv_buffer_, bytes_transferred);

std::cout << "OnReceive 2 remain_size_:" << remain_size_ << " - bytes_transferred:" << bytes_transferred << std::endl;
```

데이터 수신 후 처리하는 부분인데 단순히 packet_buffer에 memcpy를 하기만했는데 remain_size의 값이 변화한다는 사실.

위에서 출력된 remain_size와 밑에서 출력된 remain_size의 값이 다르다는 것이었다. 더 문제는 이 출력이 어떤 경우에는 정상적이고 어떤 경우에는 다르게 출력되었다. remain 값을 변화시키는게 없는데 왜 그럴까 한참 살펴보다가 VS2015의 메모리 디버거를 보다가 허탈하게 이유를 알아냈다.

remain_size가 할당된 위치가 packet_buffer의 바로 다음 메모리 공간이었다. 메모리 복사시, recv_buffer와 packet_buffer 크기가 같고 remain_size가 0보다 크다면, packet_buffer 바로 다음 메모리 공간(=remain_size가 할당된 위치)까지 메모리 복사가 일어나서 오염되었던 것.

그런데 이와 동일한 코드를 쓴 다른 서버는 같은 코드임에도 에러가 일어나지 않았다. 같은 코드더라도 프로그램마다 메모리 공간 할당이 다르게 이루어진다는 것으로 이해해야할까.
