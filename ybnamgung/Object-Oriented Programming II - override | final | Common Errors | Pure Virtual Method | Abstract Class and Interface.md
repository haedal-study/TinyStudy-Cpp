# Object-Oriented Programming II - override | final | Common Errors | Pure Virtual Method | Abstract Class and Interface

# override

**`override`** 키워드는 함수가 가상 함수이며 기본 클래스에서 가상 함수를 오버라이딩하고 있음을 보장한다.

컴파일러가 기본 클래스에서 이와 정확히 일치하는 시그니처의 가상 함수가 있는지 확인하도록 강제한다. 일치하는 가상함수가 없으면 에러가 발생한다.

# final

**`final`** 키워드는 클래스 상속이나 파생 클래스에서 메서드 오버라이딩을 방지한다.

```cpp
struct A {
		virtual void f(int a) final; // "final" method
};
struct B : A {
		// void f(int a); // compile error f(int) is "final"
		void f(float a); // dangerous (still possible)
		// "override" prevents these errors
};

struct C final { }; // cannot be extended
// struct D : C { // }; // compile error C is "final"
```

## Common Error 1

적어도 하나의 가상 메서드를 가진 모든 클래스는 가상 소멸자를 선언해야 한다.

가상함수가 있으면 부모의 소멸자가 자동으로 호출되지 않는다. 이는 메모리 누수를 유발할수도 있다.

## Common Error 2

생성자와 소멸자에서 가상 메서드를 호출하지 말아야 한다.

- 생성자 : 생성자가 완료되기 전까지 파생 클래스는 준비되지 않는다.
- 소멸자 : 파생 클래스는 이미 소멸되었다.

## Common Error 3

가상 메서드에서는 기본 매개변수를 사용하지 말아야 한다.

기본 매개변수는 상속되지 않기 때문에 예기치 않은 동작이나 버그가 발생할 수 있다.

```cpp
struct A {
		virtual void f(int i = 5) { cout << "A::" << i << "\n"; }
		virtual void g(int i = 5) { cout << "A::" << i << "\n"; }
};

struct B : A {
		void f(int i = 3) override { cout << "B::" << i << "\n"; }
		void g(int i) override { cout << "B::" << i << "\n"; }
};

A a;
B b;
a.f(); // ok, print "A::5"
b.f(); // ok, print "B::3"

A& ab = b;
ab.f(); // !!! print "B::5"  // the virtual table of A
							               // contains f(int i = 5) and
ab.g(); // !!! print "B::5"  // g(int i = 5) but it points
							               // to B implementations

```

# Pure Virtual Method

순수 가상 메서드는 파생 클래스에서 반드시 구현해야 하는 함수이다 (구체적인 구현).

순수 가상 함수는 본문을 가질 수도 있고 가지지 않을 수도 있다. 하지만 본문을 가지고 있다고 해도 무조건 파생 클래스는 구현해야 한다.

```cpp
struct A {
		virtual void f() = 0; // pure virtual without body
		virtual void g() = 0; // pure virtual with body
};

void A::g() {} // pure virtual implementation (body) for g()

struct B : A {
		void f() override {} // must be implemented
		void g() override {} // must be implemented
};
```

클래스가 순수 가상 메서드를 하나라도 가지고 있다면 인스턴스를 생성할 수 없다.

# Abstract Class and Interface

- 순수 가상 함수만 가지고 있고 선택적으로 (권장됨) 가상 소멸자를 가지는 클래스는 인터페이스이다. 인터페이스는 구현이나 데이터를 가지지 않는다.
- 최소 하나의 순수 가상 함수를 가지고 있는 클래스는 추상 클래스이다.