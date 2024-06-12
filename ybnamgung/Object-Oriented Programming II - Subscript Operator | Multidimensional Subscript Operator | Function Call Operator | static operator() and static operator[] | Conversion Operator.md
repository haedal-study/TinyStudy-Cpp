# Subscript Operator
배열 서브스크립트 `operator[]`는 배열과 같은 방식으로 객체에 접근할 수 있게 한다. 이 연산자는 정수뿐만 아니라 모든 것을 매개변수로 받아들일 수 있다.
```cpp
struct A {
	char permutation[] {'c', 'b', 'd', 'a', 'h', 'y'};

	char& operator[](char c) { // read/write
		return permutation[c - 'a'];
	}
	char operator[](char c) const { // read only
		return permutation[c - 'a'];
	}
};

A a;
a['d'] = 't';
```

# Multidimensional Subscript Operator
`C++23`에서는 다차원 서브스크립트 연산자를 도입하여 쉼표 연산자의 표준 동작을 대체한다.
```cpp
struct A {
	int operator[](int x) { return x; }
};
struct B {
	int operator[](int x, int y) { return x * y; } // C++23이전에는 사용 불가
};

int main() {
	A a;
	cout << a[3, 4]; // return 4 (bug)
	B b;
	cout << b[3, 4]; // return 12, C++23
}
```

# Function Call Operator
함수 호출 연산자 `operator()`는 일반적으로 함수처럼 동작하는 객체를 만들거나, 주요 작업을 수행하는 클래스를 위해 오버로딩된다.
```cpp
#include <numeric> // for std::accumulate

struct Multiply {
	int operator()(int a, int b) const {
		return a * b;
	}
};

int array[] = { 2, 3, 4 };
int factorial = std::accumulate(array, array + 3, 1, Multiply{});
cout << factorial; // 24
```

# static operator() and `static operator[]`
C++23에서는 this 포인터를 전달하지 않기 위해 함수 호출 연산자 operator()와 배열 첨자 연산자 operator[]의 정적 버전을 도입했다.
```cpp
#include <numeric>

struct Multiply {
	// int operator()(int a, int b); // 선언만 함
	static int operator()(int a, int b); // 가장 효율적. 멤버 데이터에 접근 필요 X
};

struct MyArray {
	// int operator[](intx);
	static int operator[](int x); // 가장 효율적.
};
int array[] = { 2, 3, 4 };
int factorial = std::accumulate(array, array + 3, 1, Multiply{});
```

# Conversion Operator `operator T()`
변환 연산자 `operator T()`는 객체를 다른 타입으로 암시적 또는 명시적으로 변환할 수 있도록 한다. 이를 통해 클래스 객체를 특정 타입으로 변환할 수 있는 기능을 제공한다.
```cpp
class MyBool {
	int x;
public:
	MyBool(int x1) : x{x1} {}

	operator bool() const { // 반환 타입 구현
		return x == 0;
	}
};

MyBool my_bool{3};
bool b = my_bool; // b = false, call operator bool()
```

C++11에서는 변환 연산자를 `explicit` 키워드로 표시하여 암시적 변환을 방지할 수 있다. 이는 클래스 생성자에서와 마찬가지로 좋은 실천 방법이다.
```cpp
struct A {
	operator bool() { return true; }
};

struct B {
	explicit operator bool() { return true; }
};

A a;
B b;
bool c1 = a;
// bool c2 = b; // compile error : explicit
bool c3 = static_cast<bool>(b);
```