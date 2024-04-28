# Basic_Concepts_4 - Source Location Macros

## Source Location Macros

- __LINE__ : 컴파일 중인 소스 코드 파일에서 현재 줄을 나타내는 정수 값
- __FILE__ : 현재 컴파일 중인 소스 파일의 이름을 포함하는 문자열 리터럴
- __FUNCTION__ : "매크로 범위"에서 함수의 이름을 포함하는 문자열 리터럴(non-standard, gcc, clang)
- __PRETTY_FUNCTION__ : "매크로 범위"에서 함수의 전체 시그니처를 포함하는 문자열 리터럴(non-standard, gcc, clang)
- __func__ : (C++11 키워드) "매크로 범위"에서 함수의 이름을 포함하는 문자열

```cpp
# include <iostream>
void f(int p) {
		std::cout << __FILE__ << ":" << __LINE__; // print 'source.cpp:4'
		std::cout << __FUNCTION__; // print 'f'
		std::cout << __func__; // print 'f'
}

// see template lectures
template<typename T>
float g(T p) {
		std::cout << __PRETTY_FUNCTION__; // print 'float g(T) [T = int]'
		return 0.0f;
}

void g1() { g(3); }
```

C++20은 매크로 기반 접근 방식을 대체하기 위한 소스 위치 유틸리티를 제공합니다.

#include <source location>

- current() : 소스 위치 정보를 가져오는 정적 멤버
- line() : 소스 코드 줄
- column() : 라인 열
- file_name() : 현재 파일 이름
- function_name() : 현재 함수 이름