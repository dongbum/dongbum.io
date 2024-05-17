---
title: 'C#에서 엔디안 변경'
date: 2013-10-02T13:47:00+09:00
categories:
  - 'C#'
tags:
  - byte order
  - C
  - endian
  - 네트워크
  - 리틀엔디안
  - 바이트오더
  - 빅엔디안
  - 엔디안
---
C#에서 네트워크 통신을 할게 있어서 바이트오더를 빅엔디안으로 해주려다가 알게 된게 있어서 정리한다.

일단 바이트오더링을 하기 위해 리틀엔디안-빅엔디안의 변환이 필요한데 C#에는 이를 지원하는 메서드가 이미 있었다.

<http://msdn.microsoft.com/en-us/library/fw3e4a0f> 에 있는 `HostToNetworkOrder` 와 `NetworkToHostOrder` 라는 메서드인데 이상한건 이 메서드들이 int16, int32, int64만 지원한다는 것이다. 난 uint16, uint32를 변경해야했기에 아무리 해봐도 이 메서드를 통해서는 바이트오더를 변경할 수 없었다. 강제로 형변환도 해봤지만 데이터가 잘못 들어가기만 했다.

구글을 한참 뒤져서 스택오버플로우에서 답을 찾았다.

<http://stackoverflow.com/questions/11232594/c-sharp-reversed-ushort>

위 링크의 리플처럼 다음의 코드를 사용하면 된다.

```csharp
var arr = new byte[1];

arr = BitConverter.GetBytes(total_length);
Array.Reverse(arr);
arr.CopyTo(buffer, offset);
offset += Marshal.SizeOf(total_length);
```

원리는 바이트배열을 선언하고 비트컨버터를 통해 바이트를 구한다음 `Array.Reverse()`를 통해 바이트를 거꾸로 하는 것이다. 한마디로 바이트오더링을 직접 구현한 것이라고 보면 될 것 같다.

여튼 이 코드를 통해 테스트해보니 정상작동함을 확인했다.
