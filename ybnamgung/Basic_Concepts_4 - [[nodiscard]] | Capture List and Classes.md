# Basic_Concepts_4 - [[nodiscard]] | Capture List and Classes

## [[nodiscard]]

C++23에서는 람다식에 [[nodiscard]] 속성을 추가할 수 있다.

```cpp
auto lambda = [] [[nodiscard]] (){ return 4; };
lambda(); // 컴파일러 경고
auto x = lambda();
```

## Capture List and Classes

- [this] 현재 객체(*this)를 참조로 캡처한다. (C++17에서는 암시적)
- [x = x] 객체 멤버 x를 값으로 캡처한다. C++14
- [&x = x] 현재 객체의 멤버 x를 참조로 캡처한다. C++14
- [=] 기본적으로 this 포인터를 값으로 캡처하는 것이 더 이상 권장되지 않는다. C++20

```cpp
class A{
    int data = 1;
    
    void f()
    {
        int var = 2; // <-- 지역 변수
        
        auto lambda1 = [=]() {return var;}; // 값복사, 2반환
        auto lambda2 = [=]() { int var = 3; return var;}; // 3 반환
        auto lambda3 = [this]() {return data;}; // 참조복사, 1반환
        auto lambda4 = [*this]() {return data;}; // 값복사(C++17), 1반환
        // auto lambda5 = [data]() {return data;}; // 컴파일 에러, 'data'를 찾을 수 없다
        auto lambda6 = [data = data]() {return data;}; // 1반환
    }
};
```