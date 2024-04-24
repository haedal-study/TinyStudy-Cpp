# Basic_Concepts_3 - Default Parameters | Attributes [[attribute]]

## Default/Optional parameter

Default Parameter는 함수 파라미터에 기본 값을 설정한다.

- 유저가 이 파라미터에 값을 넣지않으면, 기본값이 사용된다.
- 모든 디폴트 파라미터는 파라미터 목록의 가장 오른쪽에 있어야 한다.
- 모든 디폴트 파라미터는 한 번만 선언되어야 한다.
- 디폴트파라미터는 컴파일 시간을 개선하고, 다른 오버로드된 함수를 정의하는것을 피하며 중복 코드를 피할 수 있다.

```cpp
void f(int a, int b = 20);

// void f(int a, int b = 10); // 컴파일 에러. 이미 존재하며, 디폴트 파라미터 재정의

// void f(int a, int b); // 컴파일 에러. 이미 존재한다.

f(5); // b는 20
```

## Attributes [[attribute]]

C++에선 함수의 의도를 더 명확하게 표현하기 위해 표준 프로퍼티로 함수를 표현할 수 있다.

- C++11 [[noreturn]] : 함수가 최적화 목적이나 컴파일러 경고를 위해 반환하지 않음을 나타낸다.
- C++14 [[deprecated]], [[deprecated(”reason”)]] : 함수의 사용이 권장되지 않음을 나타낸다. 사용하면 컴파일러 경고가 발생한다.
- C++17 [[nodiscard]], C++20 [[nodiscard(”reason”)]] : 함수의 반환값을 무시하면 컴파일러 경고가 발생한다.

```cpp
[[deprecated]] void my_rand() {}

[[deprecated("please use rnd()")]] void my_rand2() {}

[[nodiscard]] int f() { return 3; }

[[noreturn]] void g() { std::exit(0); }

my_rand(); // 경고
my_rand2(); // 경고 "please use rnd()"
f(); // 경고
int z = f(); // 경고 없음
g(); // 이 함수를 호출한 후에는 코드가 없다.
```