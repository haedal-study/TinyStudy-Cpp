# Object-Oriented Programming I - RAII Idiom

# RAII Idiom

RAII 규칙의 세 가지 단계

1. 클래스에 자원을 캡슐화 한다.(생성자)
2. 클래스의 지역 인스턴스를 통해 자원을 사용한다.
3. 객체가 범위를 벗어나면 자동으로 해제된다.(소멸자)

C++ 언어는 가비지 컬렉터를 필요로 하지 않는다.

프로그래머는 리소스를 관리하는 책임이 있다.

RAII(Resource Acquisition Is Initialization)

C++ 프로그래밍에서 중요한 디자인 패턴중 하나로, 자원 획득과 초기화를 객체의 생성 시점에 수행함으로써 누수를 막는 기법. 객체를 생성할때 자원을 획득하고 객체가 소멸될때 자원을 해제한다. 이로써 메모리 누수와 같은 문제를 방지할 수 있으며, 코드의 안정성을 높일 수 있다.

# struct/class 선언과 정의

```cpp
struct A; // 구조체 선언

struct A { // 구조체 정의
		int x; // 데이터 멤버
		void f(); // 함수 멤버
};

class B; // 클래스 선언

class B { // 클래스 정의
		int x; // 데이터 멤버
		void f(); // 함수 멤버
};
```

# struct/class 함수 선언과 정의

```cpp
struct A {
		void g(); // 함수 멤버 선언
		
		void f() { // 함수 멤버 선언
				cout << "f"; // 인라인 정의
		}
};

void A::g() { // 함수 멤버 정의
		cout << "g"; // 아웃 오브 라인 정의
}
```

# struct/class 멤버

```cpp
struct B {
		void g() { cout << "g"' } // 함수 멤버
};

struct A {
		int x; // 데이터 멤버
		B b; // 데이터 멤버
		void f() { cout << "f"; } // 함수 멤버
};

A a;
a.x;
a.f();
a.b.g();
```