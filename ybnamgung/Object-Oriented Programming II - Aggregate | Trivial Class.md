# Aggregate
---
집합체는 중괄호 구문 {}을 통한 목록 초기화를 지원하는 배열, 구조체 또는 클래스를 말한다.

집합체는 특정 조건을 충족해야 한다.
- 사용자 제공 생성자가 없어야 한다.
- 모든 비정적 데이터는 public으로 선언되어야 한다.
- 가상함수가 없어야 한다.
- 부모 클래스가 없어야 한다.(C++17 이전까지)
- 비정적 데이터 멤버에 중괄호 혹은 등호 초기화를 사용할 수 없었다(C++14까지)
- 기반 클래스의 비정적 데이터 멤버에도 재귀적으로 적용된다.
제한 없음
- 초기화하지 않은 비정적 데이터 및 함수(C++14까지)
- 정적 데이터와 함수 멤버
```cpp
struct Aggregate {
	int x; // ok, public member
	int y[3]; // ok, arrays are also fine
	int z { 3 } // only C++14

	Aggregate() = default; // ok, default contructor
	Aggregate& operator=(const& Aggregate); // ok, function copy-assignment
private:
	void f() {} // ok, private function
};

struct NonAggregate1 {
	NotAggregate1(); // !! user-provided constructor
	virtual void f(); // !! virtual function
};
class NotAggregate2 : NotAggregate1 { // !! the base class is not an aggregate
	int x; // !! x is private
	NotAggregate1 y; // !! y is not an aggregate (recursive property)
};
```

```cpp
struct Aggregate1 {
	int x;
	struct Aggregate2 {
		int a;
		int b[3];
	} y;
};

int array1[3] = {1, 2, 3};
int array2[3] {1, 2, 3};
Aggregate1 agg1 = {1, {2, {3, 4, 5}}};
Aggregate1 agg2 {1, {2, {3, 4, 5}}};
Aggregate1 agg3 = {1, 2, 3, 4, 5};
```

# Trivial Class
---
Trivial Class는 복사 가능한 클래스를 의미한다.(memcpy 지원)

Tribial 복사 가능 클래스의 조건
- 사용자 제공 복사/이동/기본 생성자, 소멸자, 복사/이동 할당 연산자가 없어야 한다.
- 가상 함수가 없어야 한다.
- 조건이 기본 클래스, 비정적 멤버 데이터에 재귀적으로 적용된다.
제약 없음
- 복사/이동/기본 생성자와 다른 사용자 정의 생성자를 가질 수 있다
- 함수, 정적/비정적 멤버를 초기화할 수 있다
- `protected` / `private`멤버를 가질 수 있다
```cpp
struct NonTrivial {
	NonTrivial(); // !! user-provided constructor
	virtual void f(); // !! virtual function
};

struct Trivial1 {
	Trivial1() = default; // ok, defualt constructor
	Trivial1(int) {} // ok, user-default contructor
	static int x; // ok, static member
	void f(); // ok, function
private:
	int z { 3 } // ok, private and initialized
};

struct Trivial2 : Trivial1 { // ok, base class is trivial
	int Trivial1[3]; // ok, array of trivials is trivial
}
```