# Basic_Concepts_4 - Token-Pasting Operator ## | Variadic Macro

## Token-Pasting Operator ##

토큰 매크로 연산자(##)는 두 토큰을 공백없이 결합할 수 있도록 한다.

```cpp
# define FUNC_GEN_A(tokenA, tokenB) \
    void tokenA##tokenB() {}
# define FUNC_GEN_B(tokenA, tokenB) \
    void tokenA##_##tokenB() {}

FUNC_GEN_A(my, function)
FUNC_GEN_B(my, function)

myfunction();
my_function();
```

## Variadic Macro

C++11의 가변 매크로는 변수 개수의 인수를 쉼표로 구분됨을 허용하는 특별한 매크로이다.
`__VA_ARGS__`라는 특별한 식별자의 각 발생은 전달된 인수로 대체된다.

```cpp
void f(int a) {printf("%d", a);}
void f(int a, int b) {printf("%d %d", a, b);}
void f(int a, int b, int c) {printf("%d %d %d", a, b, c);}

#define PRINT(...) \
    f(__VA_ARGS__);
    
PRINT(1, 2)
PRINT(1, 2, 3)
```