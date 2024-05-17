---
title: 가변인자를 string 으로 사용하기
date: 2019-07-01T11:30:30+09:00
categories:
  - C/C++
---
C/C++에는 함수의 인자를 가변으로 다룰 수 있도록 되어있는데 이 때 사용하는 것이 `va_list`, `va_start`, `va_arg`, `va_end` 이렇게 네가지 함수다. 네가지 함수에 대한 설명은 이미 인터넷에 많이 나와있다. 그런데 이 가변인자를 문자열 형태로 사용하려고 하면 문제가 많아서 이를 테스트하는 코드를 만들었다.

```cpp
void Test(const char* types, ...)
{
    va_list ap;

    va_start(ap, types);

    while (types)
    {
        // std::cout << code :  << types << std::endl;
        types = va_arg(ap, const char*);
    }

    va_end(ap);
}

#define Test(...) Test(__VA_ARGS__, NULL)

int main()
{
    Test(A123, A234, A345);

    return 0;
}
```

가변인자로 넣을 때는 `std::string` 으로 하려고 했으나 이 타입이 먹히지 않아 `const char*` 로 받아야한다. 중간에 define 으로 함수와 같은 이름의 매크로를 선언하고 가변인자의 맨 뒤에 NULL 을 넣어주도록 했는데 이게 없다면 가변인자의 맨 끝을 알 수 없어서 문제가 생기게 된다.

---

### 참고

  * <https://stackoverflow.com/questions/35705186/printing-a-string-using-function-with-variable-argument-va-list>
  * <https://jangsalt.tistory.com/entry/%EA%B0%80%EB%B3%80-%EC%9D%B8%EC%88%98-vastart-vaend-vaarg-valist>
