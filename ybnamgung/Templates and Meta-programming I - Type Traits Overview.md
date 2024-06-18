# Overview

Introspection은 타입을 검사하고 속성을 쿼리하는 기능이다.
Reflection은 컴퓨터 프로그램이 자신의 구조와 행동을 조사하고, 성찰하고, 수정하는 능력이다.

C++은 Type Traits를 통해 컴파일 타임 Reflection과 Introspection을 제공한다.

Type Traits는 타입의 속성을 쿼리하거나 수정하기 위한 컴파일 시간 인터페이스를 정의한다.

```cpp
template<typename T>  
T integral_div(T a, T b) {
	return a / b;
}

integral_div(7, 2); // returns 3 (int)
integral_div(7l, 2l); // returns 3 (long int)
integral_div(7.0, 3.0); // !!! a floating-point value is not an integral type
```

위의 문제를 해결하는 방법은 두 가지가 있다.
1. 특수화
2. Type Traits + `static_assert`

만약 부동소수점과 그 외의 객체의 나눗셈을 컴파일 타임에 막고 싶다면, 첫 번째 대안으로는 모든 정수형 타입을 특수화 하는 방법이 있다.

```cpp
template<typename T>  
T integral_div(T a, T b); // declaration (error for other types)

template<>  
char integral_div<char>(char a, char b) {
	return a / b;
}

template<>  
int integral_div<int>(int a, int b) {
	return a / b;
}  
...unsigned char
...short  
...
```

가장 좋은 방법은 Type Traits를 사용하는 것이다.

```cpp
#include <type_traits> // <-- std type traits library
template<typename T>  
T integral_div(T a, T b) {
	static_assert(std::is_integral<T>::value,
		"integral_div accepts only integral types");
	return a / b;
}
```

`std::is_integral<T>`는 T가 bool, char, short, int, long, long long, false이면 static constexpr boolean 필드 값이 true인 구조입니다

C++17은 Type Traits 의 가독성을 향상시키는 유틸리티를 제공한다.
```cpp
std::is_integral_v<T>; // std::is_integral<T>::value
```