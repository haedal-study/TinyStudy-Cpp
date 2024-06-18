# Type Manipulation
---

Type Traits를 사용하면 `type`필드를 사용하여 타입을 조작할 수 있다.

`int`로부터 `unsigned` 생성
```cpp
#include <type_traits>

using U = typename std::make_unsigned<int>::type;

U y = 5; // unsigned
```

C++14는 Type Triats의 가독성을 향상시키는 유틸리티를 제공한다.
```cpp
std::make_unsigned_t<T>; // instead of 'typename std::make_sungined<T>::type'
```

## signed과 unsigned 타입

- `make_signed` : signed 타입으로 만든다.
- `make_unsigned` : unsigned 타입으로 만든다.

## Pointers와 References

- `remove_pointer` : 포인터 제거(T* -> T)
- `remove_reference` : 참조 제거(T& -> T)
- `add_pointer` : 포인터 추가(T -> T*)
- `add_lvalue_reference` : 참조 추가(T -> T&)

`const` specifier
- `remove_const` : const 제거(const T -> T)
- `add_const` : const 추가

다른 타입 전환
- `common_type<T, R>` : T와 R 사이의 일반 타입 반환
- `conditional<pred, T, R>` : pred가 true면 T를 반환하고, 그게 아니라면 R반환
- `decay<T>` : 함수의 값타입 매개변수와 같은 타입을 반환

# 예제

```cpp
# include <type_traits>

template<typename T>
void f(T ptr) {
	using R = std::remove_pointer_t<T>;
	R x = ptr[0]; // char
}

template<typename T>
void g(T x) {
	using R = std::add_const_t<T>;
	R y = 3;  
	// y = 4; // compile error
}

char a[] = "abc";
f(a); // T: char*
g(3); // T: int
```

```cpp
# include <type_traits>

template<typename T, typename R>
std::common_type_t<R, T> // <-- return type
add(T a, R b) {
	return a + b;
}

// we can also use decltype to derive the result type
using result_t = decltype(add(3, 4.0f));  
result_t x = add(3, 4.0f);
```

```cpp
# include <type_traits>

template<typename T, typename R>
auto f(T a, R b) {
	constexpr bool pred = sizeof(T) > sizeof(R);
	using S = std::conditional_t<pred, T, R>;
	return static_cast<S>(a) + static_cast<S>(b);
}

f(2, 'a'); // return 'int'  
f(2, 2ull); // return 'unsigned long long'  
f(2.0f, 2ull); // return 'unsigned long long'
```