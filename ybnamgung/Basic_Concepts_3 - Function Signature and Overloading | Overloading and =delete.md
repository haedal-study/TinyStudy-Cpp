# Basic_Concepts_3 - Function Signature and Overloading | Overloading and =delete

## Function Signature and Overloading

### Signature

Function signature는 (특수화된) 함수의 입력 타입들을 정의하며, 템플릿 함수의 입력과 출력 타입들을 정의한다.

함수의 시그니처에는 인자의 수, 인자의 타입, 그리고 인자의 순서가 포함된다.

- C++ 표준은 반환 타입만 다른 함수 선언을 금지한다.
- 다른 시그니처를 가진 함수 선언은 다른 반환 타입을 가질 수 있다.

### Overloading

Function overloading은 동일한 이름을 가진 다른 시그니처를 가진 함수들을 구별하여 사용할 수 있게 한다.

```cpp
void f(int a, char* b); // 시그니처 : (int, char*)

// char f(int a, char* b); // 컴파일 에러 : 같은 시그니처이지만 반환 타입이 다름.

void f(const int a, char* b); // 같은 시그니처. (const int == int)

void f(int a, const char* b); // 오버로딩 (int, const char*)

int f(float); // 오버로딩 (float) 반환 타입이 다르다.
```

### Overloading Resolution Rule

> 여러개의 오버로딩된 함수 중에 어떤 함수가 호출될지 결정하는 규칙
> 
- 정확한 매치 : 인자의 수와 타입이 완벽하게 일치하는 함수가 우선적으로 선택된다.
- 프로모션 : 인자타입이 정확하게 일치하지 않을때, 더 작은 크기의 데이터 타입이 더 큰 크기의 데이터 타입으로 자동변환. (char → int)
- 표준 타입 변환 : 인자의 타입이 정확하게 일치하지 않을경우, 표준 타입 변환을 통해 일치하는 타입으로 변환.
- 생성자 혹은 사용자 정의 타입 변환 : 변환 생성자 또는 타입 변환 연산자를 통해 구현된 사용자 정의 타입으로 변환.

```cpp
void f(int a);
void f(float b); // 오버로드
void f(float b, char c); // 오버로드

f(0); // 정확한 매치
f('a'); // 프로모션
// f(3LL); // 컴파일 에러
f(2.3f); // 정확한 매치
// f(2.3); // 컴파일 에러
f(2.3, 'a'); // 표준 타입 변환.
```

## Overloading and =delete

`= delete`키워드는 잘못된 오버로드를 막는데 사용할 수 있다.

```cpp
void g(int) {}
void g(double) = delete;

g(3); // 문제없음
// g(3.0); // 컴파일 에러
```

```cpp
#include <cstddef> // std::nullptr_T

void f(int*) {}
void f(std::nullptr_t) = delete;

f(nullptr); // 컴파일 에러
```