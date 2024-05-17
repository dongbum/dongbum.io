다음과 같은 구조체가 있을 때

```
#pragma once

#pragma pack(push, 1)

struct FRzTcpHeader
{
	int32		Size;		// packet size
	bool		bEncrypt;	// encryption
	uint16		Command;	// command
};

#pragma pack(pop)
```


아래의 코드를 입력하면 7이 출력된다.

```
std::cout << sizeof( FRzTcpHeader ) << std::endl;
```

그런데 이 값을 음수와 비교하면,

```
std::cout << ( -1 < static_cast<int32>( sizeof( FRzTcpHeader ) ) ) << std::endl;
```

이 결과는 false 가 된다.

-1 < 7 인데도 결과가 true가 아니고 false 인 것.


타입 캐스팅을 해야만 원하는 결과에 다다른다.

```
std::cout << ( -1 < static_cast<int32>( sizeof( FRzTcpHeader ) ) ) << std::endl;
```
