# Object-Oriented Programming I - const | mutable | using | friend | delete

# Const

const member function (inspectors or observers)는 객체의 논리적 상태를 변경할 수 없도록 const로 표시된 함수이다.

컴파일러는 observer function에서 데이터 멤버가 변경되는 것을 방지한다 → observer function 내에서는 모든 데이터 멤버가 const로 표시되며, this 포인터도 포함된다.

- const 접미사가 없는 멤버 함수는 non-const 멤버 함수 또는 변경자(mutators)/수정자(modifiers)라고 불린다.

const member function이 유용한 일반적인 경우는 포인터에 접근할 때 const-correctness를 강제하는 것이다.

<aside>
💡 **논리적 상태와 물리적 상태**
- 논리적 상태는 외부에서 관측할 수 있는 상태를 말한다.
- 물리적 상태는 외부에서 관측할 수 없거나 관측하더라도 직접적인 값이 아닌 어떠한 연산을 거친 후 값을 반환하는 상태를 말한다.
두 상태를 명확하게 구분할 수는 없다.

</aside>

**`const`** 키워드는 함수 시그니처의 일부이다. 따라서, 클래스는 객체가 **`const`**일 때 호출되는 메서드와 그렇지 않을 때 호출되는 두 개의 유사한 메서드를 구현할 수 있다.

```cpp
class A{
		int x = 3;
public:
		int& get1() { return x; } // read and write
		int get1() const { return x; } // read only
		int& get2() { return x; } // read and write
};

A a1;
cout << a1.get1(); // ok
cout << a1.get2(); // ok
a1.get1() = 4; // ok
const A a2;
cout << a2.get1(); // ok <- int get1() const 호출
// cout << a2.get2(); // error a2는 const 객체임.
// a2.get1() = 5; // error get1은 읽기만 가능
```

# mutable

`const` 클래스 인스턴스의 `mutable` 데이터 멤버는 수정가능하다. 이들은 객체의 물리적 상태에 속하지만 논리적 상태에는 속하지 않아야 한다.

- 대부분의 멤버가 상수여야 하지만 일부는 수정이 필요할 때 유용하다.
- 개념적으로, **`mutable`** 멤버는 클래스 인터페이스에서 가져올 수 있는 것을 변경해서는 안 된다.

```cpp
struct A {
		int x = 3;
		mutable int y = 5;
};

const A a;
// a.x = 3; // compile error
a.y = 5; // ok
```

# using

using 키워드는 특정 클래스에 바인딩된 타입 별칭을 선언하는 데 사용된다.

```cpp
struct A {
		using type = int;
};

typename A::type x = 3;

struct B : A {};

typename B::type x = 4; // type이 public이라면 상속받아도 사용할 수 있다.
```

`using` 키워드는 멤버 데이터나 함수의 상속 속성을 변경하는 데에도 사용할 수 있다.

```cpp
struct A {
protected:
		int x = 3;
};

struct B : A {
public:
		using A::x;
};

B b;
b.x = 3; // ok
```

# friend

`friend` 클래스는 해당 클래스가 `friend`로 선언된 클래스의 개인 및 보호된 멤버에 액세스할 수 있다.

`friend`의 특성

- 대칭적이지 않음 : 만약 클래스 A가 클래스 B의 친구일때 클래스 B가 자동으로 클래스 A의 되지는 않는다.
- 전이적이지 않음 : 만약 클래스 A가 클래스 B의 친구이고, 클래스 B가 클래스 C의 친구일때 클래스 A가 자동으로 클래스 C의 친구가 되지는 않는다.
- 상속되지 않음 : 만약 클래스 Base가 클래스 X의 친구라면, 하위클래스 Derived가 자동으로 클래스 X의 친구가 되지는 않는다. 만약 클래스 X가 클래스 Base의 친구라면, 클래스 X가 자동으로 하위 클래스 Derived의 친구가 되지는 않는다.

```cpp
class B; // 클래스 선언

class A {
		friend class B;
		int x;
};

class B {
		int f(A a) { return a.x; } // ok B는 A의 친구이다.
};

class C : B {
// int f(A a) { return a.x; } // compile error
};
```

비멤버 함수는 해당 클래스의 `friend`로 선언된 경우에만 해당 클래스의 개인 및 보호된 멤버에 액세스할 수 있다.

```cpp
class A {
		int x = 3;
		
		friend int f(A a); // 친구 선언. 구현 필요 없음.
};

// 비멤버 함수
int f(A a) {
		return a.x; // 함수 f는 A의 친구이다.
}
```

`friend` 함수는 주로 스트림 연산자 operator<<를 구현하는 데 사용된다.

# delete

delete 키워드는 멤버 함수를 명시적으로 삭제하며, 해당 함수를 사용하는 모든 곳에서 컴파일러 오류를 발생시킨다. 복사/이동 생성자 또는 할당에 적용되면, 이러한 함수들이 컴파일러에 의해 암시적으로 생성되는 것을 방지한다.

클래스의 기본 복사/이동 함수는 예기치 않은 결과를 초래할 수 있다. delete 키워드는 이러한 오류를 방지한다.

```cpp
struct A {
		A() = default;
		A(const A&) = delete; // 비싸거나 안전하지 않기 때문에 지운다
};
void f(A a) {} // 함수 호출시 값복사 발생
A a;
f(a); // compile error
```