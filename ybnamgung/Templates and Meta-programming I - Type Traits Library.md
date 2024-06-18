# Type Traits Library
---
## 기본타입, 스칼라 타입

- `is_integal` : 정수형 타입 체크 (bool, char, unsigned char, short, int, long, 등)
- `is_floating_point` : 부동 소수점 타입 체크(float, double)
- `is_arithmetic` : 정수형, 부동 소수점 체크
- `is_signed` : signed 타입 체크
- `is_unsigned` : unsigned 타입 체크
- `is_enum` : enumerator 타입 체크 (enum, enum class)
- `is_void` : void 체크
- `is_pointer` : pointer 체크
- `is_null_pointer` : nullptr 체크 (C++14)
## 참조, 함수, 객체

- `is_reference` : 참조 체크
- `is_array` : 배열 체크
- `is_function` : 함수 체크
- `is_class` : 클래스 체크(struct, class)
- `is_abstract` : 최소 하나의 순수 가상 함수를 가지고 있는지 체크
- `is_polymorphic` : 최소 하나의 가상 함수를 가지고 있는지 체크

## 관계

- `is_const` : 상수 체크
- `is_same<T, R>`: T와 R이 같은 타입인지 체크
- `is_base_of<T, R>` : T가 R의 부모인지 체크
- `is_convertible<T, R>` : T가 R로 변환될 수 있는지 체크

# 예제

```cpp
# include <type_traits>  
template<typename T>  
void f(T x) { cout << std::is_const_v<T>; }

template<typename T>  
void g(T& x) { cout << std::is_const_v<T>; }

template<typename T> void h(T& x) {
	cout << std::is_const_v<T>;
	x = nullptr; // ok, it compiles for T: (const int)*
}

const int a = 3;  
f(a); // print false, "const" drop in pass by-value
g(a); // print true  
const int* b = new int;  
h(b); // print false!! T: (const int)*
```

```cpp
# include <type_traits>
template<typename T, typename R>
T add(T a, R b) {
	static_assert(std::is_same_v<T, R>, "T and R must have the same type"); 
	return a + b;
}

add(1, 2); // ok  
// add(1, 2.0); // compile error, "T and R must have the same type"
```

```cpp
# include <type_traits> struct A {};  
struct B : A {};

std::is_base_of_v<A, B>; // true
std::is_convertible_v<int, float>; // true
```