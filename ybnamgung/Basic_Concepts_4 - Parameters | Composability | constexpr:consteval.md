# Basic_Concepts_4 - Parameters | Composability | constexpr/consteval

## Parameters

C++14에서는 람다식의 파라미터가 자동으로 추론될 수 있다.

```cpp
auto x = [](auto value) { return value + 4; };
```

C++14에서는 람다식의 파라미터를 초기화할 수 있다.

```cpp
auto x = [](int i = 6) { return i + 4; };
```

## Composability

람다식은 합성될 수 있다.

```cpp
auto lambda1 = [](int value) { return value + 4;};
auto lambda2 = [](int value) { return value * 2;};
    
auto lambda3 = [&](int value) { return lambda2(lambda1(value));};
```

함수는 람다를 반환할 수 있다. (동적 디스패치도 가능하다)

```cpp
auto f() {
    return [](int value){return value + 4;};
}

auto lambda = f();
cout << lambda(2);
```

## Constexpr/consteval

C++17에서는 람다식이 constexpr을 지원한다.

C++20에서는 람다식이 consteval을 지원한다.

```cpp
auto factorial = [](int value) constexpr {
    int ret = 1;
    for (int i = 2; i <= value; i++) ret *= i;
    return ret;
};
auto mul = [](int v) consteval { return v * 2; };
constexpr int v1 = factorial(4) + mul(5);
```