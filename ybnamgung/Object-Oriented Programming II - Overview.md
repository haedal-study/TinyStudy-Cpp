# Object-Oriented Programming II - Overview

# Polymorphism

객체지향 프로그래밍(OOP)에서 다형성은 "여러 형태를 가지는 것"을 의미하며, 객체가 특정 사용 상황에 따라 동작을 변형할 수 있는 것을 말한다.

- 런타임에 기반 클래스의 객체는 파생 클래스의 객체처럼 동작한다.
- 기반 클래스는 다형성 메서드를 정의하고 구현할 수 있으며, 파생 클래스는 이를 오버라이드하여 자신만의 구현을 제공할 수 있다. 이러한 구현은 상황에 따라 런타임에 호출된다.

## Polymorphism vs Overloading

오버로딩은 정적 다형성(컴파일 타임 다형성)의 한 형태이다. C++에서 다형성이라는 용어는 동적 다형성(overriding)과 강하게 연관된다.

```cpp
// overloading example
void f(int a) {}

void f(double b) {}

f(3); // calls f(int)
f(3.3); // calls f(double)
```

## Function Binding

함수 호출을 함수 본체와 연결하는것을 바인딩(Binding)이라고 한다.

- 이른 바인딩(Early Binding) 또는 정적 바인딩(Static Binding) 또는 컴파일 타임 바인딩(Compile-time Binding)은 컴파일 타임에 객체의 타입을 식별한다.
    - 프로그램은 함수 주소로 직접 점프할 수 있다.
- 늦은 바인딩(Late Binding) 또는 동적 바인딩(Dynamic Binding)또는 런타임 바인딩(Run-time Binding)은 실행 시간에 객체의 타입을 식별하고 나서 함수 호출을 올바른 함수 정의와 매칭시킨다.
    - 프로그램은 포인터에 저장된 주소를 읽은 다음 그 주소로 점프해야 하므로 추가적인 간접 참조 단계가 필요해 덜 효율적이다.

C++은 가상함수(`virtual` function)를 선언하여 지연 바인딩을 구현한다.

## Polymorphism - The problem

```cpp
struct A {
		void f() { cout << "A"; }
};

struct B : A {
		void f() { cout << "B"; }
};

void g(A& a) { a.f(); } // A와 B를 받아준다
void h(B& b) {b.f(); } // B만 받아준다

A a;
B b;
g(a); // print "A"
g(b); // print "A" not "B"!!!
```