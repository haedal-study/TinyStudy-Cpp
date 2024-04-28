# Basic_Concepts_4 - Common Errors

## #include 이전에 헤더 파일에서 매크로를 정의하자

```cpp
# include <iostream>
#define value // 매우 위험하다
#include "big_lib.hpp"
int main() {
		std::cout << f(4); // 7이 출력되어야 하지만 언제나 3이 출력된다.
}
```

big_lib.hpp:

```cpp
int f(int value) { // 'value' 가 사라진다
		return value + 3;
}
```

매크로를 헤더 파일에 정의하면 이런 문제를 보기 힘들다.

## #if defined은 매크로 가시성 관련 버그를 발생시킬 수 있다.

```cpp
// #include "macro_definition.hpp" // ENABLE_DEBUG가 정의된 헤더파일을 추가하는것을 잊음

#if defined(ENABLE_DEBUG)
		void f(int v) { cout << v << endl; return v * 3; }
#else
		void f(int v) { return v * 3; }
#endif
```

```cpp
#if ENABLE_DEBUG // 평가시 0 혹은 1 인경우
		void f(int v) { cout << v << endl; return v * 3; }
#else
		void f(int v) { return v * 3; }
#endif
```

## 매크로 정의에서 괄호를 사용하는 것을 잊음

```cpp
#include <iostream>

#define SUB1(a, b) a - b // 틀림
#define SUB2(a, b) (a - b) // 틀림
#define SUB3(a, b) ((a) - (b)) // 맞음

int main() {
    std::cout << (5 * SUB1(2, 1)); // 5가 아닌 9 출력
    std::cout << SUB2(3 + 3, 2 + 2); // 2가 아닌 6 출력
    std::cout << SUB3(3 + 3, 2 + 2); // 2 출력
    
    return 0;
}
```

## 매크로는 컴파일 오류를 찾기 어렵게 만든다

```cpp
#include <iostream>

#define F(a) { \
					 ... \
					 ... \
					 return v;

int main() {
	F(3); // 9번째 줄에서 코드 에러
}
```

어디에서 에러가 났는지 알 수 없다.(요즘 컴파일러는 매크로를 해체할 수 있다)

## 매크로는 표현식 평가와 관련된 버그를 발생시킬 수 있다.

```cpp
#if defined(DEBUG)
	#define CHECK(EXPR) // EXPR과 함께 무언가 수행한다.
	void check(bool b) { /* b와 함께 무언가 수행한다. */ }
#else
	#define CHECK(EXPR) // 아무것도 안한다.
	void check(bool) {} // 아무것도 안한다.
#endif
bool f() { /* boolean 값을 반환 */ }
check( f() )
CHECK( f() ) // <-- 문제 발생
```

`DEBUG`가 정의되지 않으면 `f()`는 매크로를 통해 평가되지 않는다.

## 다중 라인 매크로에서 중괄호를 잊어버린경우

```cpp
# include <iostream>
# include <nuclear_explosion.hpp>

#define NUCLEAR_EXPLOSION \ // {
        std::cout << "start nuclear explosion"; \
        nuclear_explosion(); // }

int main() {
    bool never_happen = false;
    if (never_happen) NUCLEAR_EXPLOSION
    // 핵이 터진다.
}

```

## 매크로는 scope를 가지지 않음

```cpp
#include <iostream>

void f() {
		#define value 4
		std::cout << value;
}

int main() {
		f(); // 4
		std::cout << value; // 4
		#define value 3
		f(); // 4
		std::cout << value; // 3
}
```

## 매크로는 사이드 이펙트를 가질 수 있다

```cpp
#define MIN(a, b) ((a) < (b) ? (a) : (b))

int main()
{
		int array1[] = {1, 5, 2};
		int array2[] = {6, 3, 4};
		int j = 0;
		int i = 0;
		int v1 = MIN(array1[i++], array2[j++]); // v1 = 5
		int v2 = MIN(array1[i++], array2[j++]); // undefined behavior
																						// segmentation fault
```

## 매크로 자체가 undefined behavior을 가질 수 있다

```cpp
#define MY_MACRO defined(EXTERNAL_MACRO)

#if MY_MACRO
		#define MY_VALUE 1
#else
		#define MY_VALUE 0
#endif

int f() { return MY_VALUE; } // undefined behavior
```