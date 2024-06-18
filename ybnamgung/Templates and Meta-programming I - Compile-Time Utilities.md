# static_assert
---
C++11의 `static_assert`는 sizeof , literals, templates, constexpr와 마찬가지로 컴파일 타임에 어썰션을 테스트 할때 사용된다.
만약 정적 어썰션에 실패한다면 프로그램은 컴파일되지 않는다.
```cpp
static_assert(2 + 2 == 4, "test1"); // ok, it compiles
static_assert(2 + 2 == 5, "test2"); // compile error, print "test2"
```

C++17에선 메시지 없는 어썰션이 추가되었다.
```cpp
template<typename T, typename R>  
void f() { static_assert(sizeof(T) == sizeof(R)); }

f<int, unsigned>(); // ok, it compiles
// f<int, char>(); // compile error
```

C++26에선 텍스트 포멧도 지원한다.
```cpp
static_assert(sizeof(T) != 4, std::format("test1 with sizeof(T)={}", sizeof(T)));
```
# using Keyword (C++11)
---
`using`키워드를 사용하면 별칭 선언 혹은 별칭 템플릿이 가능하다.
- 보다 읽기 쉬운 구문을 가진 향상된 버전의 typedef
- typedef와 반대로 템플릿과 결합할 수 있다
- 복잡한 템플릿 표현을 단순화하는 데 유용하다
- 부분 및 전체 특수화를 위해 새 이름을 도입할 수 있습니다

```cpp
typedef int distance_t; // 다음과 동일하다
using distance_t = int;
```

```cpp
typedef void (*function)(int, float); // 다음과 동일하다
using function = void (*)(int, float);
```

일부/부분 특수화 별칭
```cpp
template<typename T, int Size>  
struct Vector {};

template<int Size>  
using Bitset = Vector<bool, Size>; // 부분 특수화 별칭

using IntV4 = Vector<int, 4>; // 전체 특수화 별칭
```

구조체 내의 타입 접근
```cpp
struct A {  
	using type = int;
};

using Alias = A::type;
```

# decltype Keyword

C++11 decltype keyword captures the type of entity or an expression
• decltype never executes, it is always evaluated at compile-type

C++11의 `decltype`키워드는 엔티티 혹은 표현의 타입을 캡처한다.
`decltype`은 절대 실행되지 않으며, 오직 컴파일 타임에만 평가된다.

## decltype value

```cpp
int x = 3;
int& y = x;
const int z = 4;
int array[2];
void f(int, float);

decltype(x) d1; // int
decltype(2 + 3.0) d2; // double
decltype(y) d3; // int&
decltype(z) d4; // const int
decltype(array) d5; // int[2]
decltype(f(1, 2.0f)) d6; // void

using function = decltype(f);
```

## decltype expression

```cpp
bool f(int) { return true; }

struct A {
	int x;
};

int x = 3;
const A a{4};

decltype(x) d1; // int
decltype((x)) d2 = x; // int&

decltype(f) d3; // bool(int)
decltype((f)) d4 = f; // bool(&)(int)

decltype(a.x) d5; // int
decltype((a.x)) d6 = x; // const int
```

## decltype Keyword + Function templates

C++11

```cpp
template<typename T, typename R>
decltype(T{} + R{}) add(T x, R y) {
	return x + y;
}

unsigned v1 = add(1, 2u);
double v2 = add(1.5, 2u);
```

C++14

```cpp
template<typename T, typename R>
auto add(T x, R y) {
	return x + y;
}
```