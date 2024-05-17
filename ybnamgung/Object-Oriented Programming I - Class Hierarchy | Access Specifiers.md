# Object-Oriented Programming I - Class Hierarchy | Access Specifiers

# Class Hierarchy

다른 클래스로부터 변수와 함수를 상속받는 새로운 클래스를 파생 클래스 또는 자식 클래스라고 한다.

파생 클래스가 변수와 함수를 제공하는 가장 가까운 클래스는 부모 클래스 또는 기본 클래스라고 한다.

기본 클래스를 확장한다는 것은 기본 클래스의 특성을 유지하면서 새로운 클래스를 생성하는 것을 의미한다. 새로운 멤버를 추가할 수 있지만, 기본 클래스의 특성을 계속 유지한다.

```cpp
class DerivedClass : [<inheritance attribute>] BaseClass {
```

```cpp
struct A { // 기본 클래스
		int value = 3;
		void g() {}
};

struct B : A { // B는 A의 파생 클래스이다. (B가 A를 확장)
		int data = 4; // B는 A로부터 상속받았다.
		
		int f() { return data; }
};

A a;
B b;
a.value;
b.g();
```

```cpp
struct A {};
struct B : A {};

void f(A a) {} // 복사
void g(B b) {} // 복사

void f_ref(A& a) {} // 참조
void g_ref(B& b) {} // 참조

A a;
B b;
f(a); // 가능. f(b), f_ref(a), g_ref(b) 또한 가능
g(b); // 가능 g_ref(b)도 가능. 하지만 g(a), g_ref(a)는 불가능

A a1 = b; // 가능.  A& a2 = b 도 가능
// B b1 = a; // 컴파일에러
```

# Access Sepcifiers

접근 지정자는 후속 기본 클래스의 상속된 멤버의 가시성을 정의한다.

public, private, protected 키워드는 가시성 섹션을 지정한다.

접근 지정자는 클래스의 내부 표현에 직접 접근하는 것을 제한하여 잘못된 사용과 데이터 일관성 위배를 방지하는것을 목표로 한다.

- public : 아무런 제한이 없다.
- protected : 멤버 함수와 파생 클래스에서만 접근할 수 있다.
- private : 멤버 함수내에서만 접근할 수 있다.

struct는 기본 접근 지정자가 public이고, class는 기본 접근 지정자가 private이다.

```cpp
struct A1 {
		int value; // public
protected:
		void f1() {} // protected
private:
		void f2() {} // private
};

class A2 {
		int data; // private
};

struct B : A1 {
		void h1() { f1(); } // 가능
		// void h2() { f2(); } // 컴파일 에러
};

A1 a;
a.value; // 가능
// a.f1(); // 컴파일 에러
// a.f2(); // 컴파일 에러
```

접근 지정자는 상속에서 기본 클래스에서 특정 파생 클래스로 가시성이 전파되는 방식을 정의하는 데도 사용된다.

| 멤버 선언 | 상속 | 파생 클래스 |
| --- | --- | --- |
| public
protected
private | public | public
protected
- |
| public
protected
private | protected | protected
protected
- |
| public
protected
private | private | private
private
- |

protected/private 데이터 멤버를 사용하는 경우

- 인터페이스의 일부가 아니다. 즉, 객체의 논리적 상태에 포함되지 않는다
- const 정확성을 유지해야 한다.

public 데이터 멤버를 사용하는 경우

- 언제든지 변경될 수 있다
- 포인터와는 달리 값과 참조에 대해 const 정확성이 유지된다. 데이터 멤버가 멤버 함수보다 우선적으로 사용되어야한다.