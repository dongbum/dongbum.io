---
title: C#과 C++
date: 2020-08-26 18:14:01
categories:
  - C/C++
  - C#
tags:
  - C#
---

C++만 매일 하다가 아주 가끔씩 C#을 할 때가 있는데 이럴 때마다 이게 왜 안되지? 하다가 시간을 많이 잡아먹는 경우가 종종 생긴다.

```cpp
public void CheckAllServers( List<ServerStatus>& list )
{
  for ( auto serverStatus : list )
      serverStatus.status = CheckSocket( serverStatus.ip, serverStatus.port );
}
```

C++였다면 이렇게 사용하면 될 기능을..

```csharp
public void CheckAllServers( ref List<ServerStatus> list )
{
    for ( int i = 0; i < list.Count; ++i )
    {
        ServerStatus serverStatus = list[ i ];
        serverStatus.status = CheckSocket( serverStatus.ip, serverStatus.port );
        list[ i ] = serverStatus;
    }
}
```

이렇게 사용해야한다.

`ref` 로 받은 리스트의 원소들을 다시 `ref` 로 사용이 안되기 때문에 새로운 객체를 복사해서 만들고 다시 대입하는 번거로운 과정을 거쳐야한다.

C#이 여러가지로 C++보다 개선되어 나왔다고는 하는데 이런 면에서 보면 아직은 C++의 기능이 더 나은 느낌.
