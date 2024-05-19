# Object-Oriented Programming I - Uniform Initilization for Objects | Delegate Consturctor | explicit Keyword | [[nodiscard]] and Classes

# **Uniform Initialization for Objects**

균일 초기화 {}는 리스트 초기화라고도 하며, 데이터 타입에 관계없이 어떤 객체도 완전히 초기화하는 방법이다.

- 인수 전달시 혹은 반환시 불필요한 타입명 최소화
- "가장 성가신 파싱" 문제 해결
    - 괄호로 생성자를 호출하는 것을 함수 호출로 오해하는 문제

클래스 생성자는 절대 상속되지 않는다.
파생 클래스는 현재 클래스 생성자 전에 기본 클래스 생성자를 암시적으로 또는 명시적으로 호출해야 한다.
클래스 생성자는 최상위 기본 클래스에서 가장 파생된 클래스 순서로 호출된다.

# Delegate Constructor

대부분의 생성자는 개별 작업을 실행하기 전에 동일한 초기화 단계를 수행한다.
C++11에서 대리 생성자는 모든 초기화 단계를 수행하는 함수를 추가하여 반복적인 코드를 줄이기 위해 동일한 클래스의 다른 생성자를 호출한다.

```cpp
struct A {
		int a;
		float b;
		bool c;
		
		A(int a1, float b1, bool c1) : a(a1), b(b1), c(c1) {
				// 많은 일을 한다.
		}
		
		A(int a1, float b1) : A(a1, b1, false) {} // 대리 생성자
		A(float b1) : A(100, b1, false) {} // 대리 생성자
```

# explicit Keyword

explicit
explicit 키워드는 생성자 또는 변환 연산자(C++11)가 단일 인수 또는 중괄호 초기화자로부터 암시적 변환이나 복사 초기화를 허용하지 않음을 지정한다.

문제점 :

```cpp
struct MyString {
		MyString(int n); // n바이트만큼의 문자열을 할당한다.
		MyString(const char* p); // 리터럴 문자열로부터 초기화를 시작한다.
};
MyString string = 'a'; // 암시적 형변환으로 인해 MyString(int n)이 호출된다.
```

```cpp
struct A {
		A() {}
		A(int) {}
		A(int, int) {}
};
void f(const A&) {}

A a1 = {}; // 가능
A a2(2); // 가능
A a3 = 1; // 가능 (암시적)
A a4{4, 5}; // 가능 A(int, int)
A a5 = {4, 5}; // 가능 A(int, int)
f({}); // 가능
f(1); // 가능
f({1}); // 가능
```

```cpp
struct B {
		explicit B() {}
		explicit B(int) {}
		explicit B(int, int) {}
};
void f(const B&) {}

// B b1 = {} // 기본 생성자가 없다.
B b2(2); // 가능
// B b3 = 1; // 에러 암시적 형변환
B b4{4, 5}; // 가능 B(int, int)
// Bb5 = {4, 5} // 에러 복사 초기화
B b6 = (B) 1; // 가능 명시적 형변환
// f({}); // 에러 암시적 형변환
// f(1); // 에러 암시적 형변환
// f({1}); // 에러 복사 생성자
f(B{1}); // 가능
```

# [[nodiscard]] and Classes

C++17에서는 [[nodiscard]] 특성을 전체 클래스/구조체에 설정할 수 있다.

```cpp
[[nodiscard]] struct A {};
A f() { return A {}; }

auto x = f(); // 가능
f(); // 컴파일 에러
```

C++20에서는 생성자에 [[nodiscard]] 특성을 설정할 수 있다.

```cpp
struct A {
		[[nodiscard]] A() {}
};
void f(A) {}

A a{}; // 가능
f(A{}); // 가능
A{}; // 컴파일 경고
```