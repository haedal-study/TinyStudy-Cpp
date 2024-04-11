# std::is_constant_evaluated()
- C++20에서는 `std::is_constant_evaluated()` 기능을 지원하는데, 이는 해당 함수가 컴파일 타임에 평가되는지 평가한다.
```
#include <type_traits> // std::is_constant_evaluated

constexpr int f(int n) {
	if(std::is_constant_evaluated())
		return 0;
	return 4;
}

int x = f(3); // x = 0

int v = 3;
int y = f(v); // y = 4
```
# if consteval
- `std::is_constant_evaluated()`는 C++23의 `if consteval`이 해결할 수 있는 두 가지 문제점이 있다.
1. 런타임 파라미터로 호출되는 `constexpr` 함수에서는 `consteval` 함수를 호출하지 못한다.
```
consteval int g(int n) { return n * 3; }

constexpr int f(int n) {
	if (std::is_constant_evaluated()) // if consteval은 작동됨
		return g(n);
	return 4;
}

//f(3); // 컴파일 에러
```
2. `if constexpr(std::is_constant_evaluated())` 는 항상 true를 반환하기 때문에 버그이다.
```
constexpr int f(int x) {
	if constexpr ( std::is_constant_evaluated())  // if consteval은 이 에러를 건너 
		return 3;
	return 4;
}
```