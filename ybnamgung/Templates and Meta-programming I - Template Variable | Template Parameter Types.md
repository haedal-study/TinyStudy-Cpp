# Template Variable
---
C++14부터 템플릿 변수가 가능해졌다.
템플릿 변수는 클래스 템플릿의 특수한 경우로 간주할 수 있다.
```cpp
template<typename T>
constexpr T pi { 3.1415926535897932385 }; // 변수 템플릿

template<typename T>
T circular_area(T r) {
	return pi<T> * r * r; // pi<T>는 변수 템플릿의 인스턴스화이다.
}

circular_area(3.3f) // float
circular_area(3.3); // double
// circular_area(3); // compile error, narrowing conversion with "pi"
```

# Template Parameter Types
---
템플릿 파라미터 타입
- 정수형 타입
- enum, enum class
- 부동 소수점 타입 `C++20`
- auto `C++17`
- 클래스 리티럴 및 `concept`
- 제네릭 타입 `typename`
드물게 사용하는 타입
- 함수
- 전역 `static` 포인터 혹은 객체의 참조/포인터
- 멤버타입의 포인터
- `nullptr_t` C++14

## Generic Type Notes
```cpp
template<float V> // only in C++20
void print_float() {}

template<typename T>
void print() {
	cout << T::x << ", " << T::y;
}

struct Multi {
	static const int x = 1;
	static constexpr float y = 2.0f;
};

print<Multi>(); // print "1, 2"
```

## `auto` Placeholder
C++17부터 `auto`키워드를 사용하여 비타입는 템플릿 매개변수의 자동 추론을 도입했다.

```cpp
template<int X, int Y>
void f() {}

template<typename T1, T1 X, typename T2, T2 Y>
void g1() {} // C++17 이전

template<auto X, auto Y>
void g2() {}

f<2u, 2u>(); // X: int, Y: int
g1<int, 2, char, 'a'>(); // X: int, Y: char
g2<2, 'a'>(); // X: int, Y: char
```

## Class Template Parameter Type
C++20에서는 클리스 리터럴 타입의 비타입 템플릿 매개변수를 사용할 수 있다.
- 클래스 리터럴은 `constexpr`변수에 할당될 수 있는 클래스를 의미한다.
- 모든 기반 클래스와 비정적 데이터 멤버가 `public`이고 변경 불가능해야 한다.
- 모든 기반 클래스와 비정적 데이터 멤버가 동일한 특성을 가져야한다.
```cpp
#include <array>
struct A {
	int x;
	constexpr A(int x1) : x{x1} {}
};

template<A a>
void f() { std::cout << a.x; }

template<std::array array>
void g() { tsd::cout << array[2]; }

f<A{5}>(); // print 5
g<std::array{1,2,3}>(); // print 3
```

## Array and Pointer Types

Array and pointer
```cpp
template<int* ptr> // pointer
void g() {
	cout << ptr[0];
}

template<int (&array)[3]> // reference
void f() {
	cout << array[0];
}

int array[] = {2, 3, 4}; // global

int main() {
	f<array>(); // print 2
	g<array>(); // print 2
}
```

Class member
```cpp
struct A {
	int x = 5;
	int y[3] = {4, 2, 3};
};

template<int A::*x> // pointer to member type
void h1() {}

template<int (A::*y)[3]> // pointer to member type
void h2() {}

int main() {
	h1<&A::x>();
	h2<&A::y>();
}
```

## Function Type
Function
```cpp
template<int (*F)(int, int)> // <-- signature of "f"
int apply1(int a, int b) {
	return F(a, b);
}

int f(int a, int b) { return a + b; }
int g(int a, int b) { return a * b; }

template<decltype(f) F> // alternative syntax
int apply2(int a, int b) {
	return F(a, b);
}

int main() {
	apply1<f>(2, 3); // return 5
	apply2<g>(2, 3); // return 6
}
```