# Basic_Concepts_4 - Stringizing Operator # | #error and #warning | #pragma

## Stringzing Operator #

실제 인수를 `“`로 감싼다.

```cpp
#define STRING_MACRO(string) #string

std::cout << STRING_MACRO(hello);
```

```cpp
#define INFO_MACRO(my_func) \
{ \
		my_func; \
    cout << "call " << #my_func << " at " \
    << __FILE__ << ":" << __LINE__; \
}

void g(int) {}

INFO_MACRO(g(3))
```

## #error and #warning

`#error` “text” 지시문은 컴파일러가 이를 파싱하고 컴파일 프로세스를 중지할 때 컴파일 시간에 사용자가 지정한 오류 메시지를 발생시킨다.

C++23 `#warning` “text” 지시문은 컴파일러가 이를 파싱할때 컴파일 프로세스를 중지하지 않고 사용자가 지정한 경고 메시지를 발생시킨다.

## #pragma

#pragma 지시문은 컴파일러의 구현별 동작을 제어한다. 일반적으로 이식성이 없다.

- #pragma message “text”는 컴파일 시간에 정보 메시지를 표시한다.
- #pragma GCC diagnostic warning “-Wformat”은 GCC 경고를 비활성화한다.
- `_Pragma(<command>)` (C++11) 이 키워드는 #define에 포함될 수 있다.
- #define MY_MESSAGE _Pragma(”message(\”hello\”)”)