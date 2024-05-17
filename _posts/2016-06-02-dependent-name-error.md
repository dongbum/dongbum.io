---
title: dependent-name is parsed as a non-type, but instantiation yields a type 에러 해결 방법
date: 2016-06-02T10:35:18+09:00
categories:
  - C/C++
  - GameServer
---
다음과 같은 코드를 작성했다.

```cpp
template<typename T1, typename T2>
class ObjectMap
{
public:
    typedef boost::mutex                    Mutex;
    typedef boost::lock_guard<Mutex>      LockGuard;
    typedef std::map<T1, T2>              Map;

    bool InsertObject(T1& key, T2& object)
    {
        LockGuard lock(object_map_mutex_);
        auto result = object_map_.insert(Map::value_type(key, object));
        return result.second;
    }
}
```

대략 템플릿으로 std::map을 래핑해서 사용하려는 클래스인데 이 코드는 Visual Studio 2015 에서 아무 문제 없이 컴파일되며 실제로 작동상에도 아무 문제가 없는 코드이다.

하지만 이 코드를 gcc에서 컴파일 하려고 하면 다음과 같은 오류가 발생하며 컴파일 자체가 되질 않는다. 참 신기한 노릇... (컴파일러는 CentOS 7의 gcc 4.8.2 버전이다.)

```
/container/object_map.h:28:63: error: dependent-name 'ObjectMap<T1, T2>::Map:: value_type' is parsed as a non-type, but instantiation yields a type
   auto result = object_map_.insert(Map::value_type(key, object));
                                                               ^
/container/object_map.h:28:63: note: say 'typename ObjectMap<T1, T2>::Map:: value_type' if a type is meant</pre>
```

이 문제에 대해 한참을 찾아본 결과 다음의 주소에서 힌트를 얻을 수 있었다.

<http://stackoverflow.com/questions/6021686/c-iterator-to-stdmap>

원인은 해당하는 부분에 컴파일러가 타입을 알 수 없다는 얘기.

코드를 다음과 같이 바꾸어주었더니 gcc에서도 컴파일이 잘 된다.

```cpp
auto result = object_map_.insert(typename Map::value_type(key, object));
```

예전에도 느꼈지만 리눅스에서 빌드하는거 참 귀찮고 어렵다는거 다시 한번 느낀다.
