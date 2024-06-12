# Return Type Overloading Resolution
```cpp
struct A {
	operator float() { return 3.0f; }
	operator int() { return 2; }
};

auto f() {
	return A{};
}

float x = f();
int y = f();
cout << x << " " << y; // x = 3.0f, y = 2
```

# Increment and Decrement Operator `operator++/--`
증가 연산자 `operator++`와 감소 연산자`operator--`는 변수의 값을 한 단위씩 업데이트 하는데 사용된다.
```cpp
struct A {
	int* ptr;
	int pos;
	A& operator++() { // 전위연산 (++var)
		++ptr;
		++pos;
		return *this; // 새 값을 할당받은 원본의 참조를 반환한다.
	}
	A operator++(int a) { // 후위연산 (var++)
		A tmp = *this;
		++ptr;
		++pos;
		return tmp; // 이전 값을 복사한 객체의 값을 반환한다.
	}
}
```

# Assignment Operator `operator=`
대입 연산자 `operator=`는 이미 존재하는 객체에 다른 객체의 값을 복사하는 데 사용된다.
```cpp
#include <algorithm> // std::fill, std::copy
struct Array {
	char* array;
	int size;

	Array(int size1, char value) : size{size1} {
		array = new char[size];
		std::fill(array, array + size, value);
	}
	~Array() { delete[] array; }

	Array& operator=(const Array& x) { ... } // 다음 코드 보기
};

Array a{5, 'o'}; // ["ooooo"]
Array b{3, 'b'}; // ["bbb"]
a = b; // a = ["bbb"] <-- 목표
```

위와 같이 구현하기 위해선 두 가지 방법이 있다.
첫 번째 방법
```cpp
Array& operator=(const Array& x) {
	if  (this == &x) // 스스로에게 대입하는건지 체크
		return *this;
	delete[] array; // 자원 반환
	size = x.size; // 재 초기화
	array = new int[x.size];
	std::copy(x.array, x.array + size, array); // 깊은 복사
	return *this;
}
```
두 번째 방법
```cpp
Array& operator=(Array x) { // 값 복사
	swap(*this, x);
	return *this;
}
```
`swap` 메소드
```cpp
friend void swap(A& x, A& y) {
	using std::swap;
	swap(x.size, y.size);
	swap(x.array, y.array);
}
```
`std::swap`을 명시하는 이유는 `swap(x, y)`과 매칭되는 함수를 찾으면 `std::swap`대신 그 함수를 쓰기 때문이다.
`friend`를 사용하는 이유는 클래스의 비멤버 함수에 접근할 수 있도록 하기 위함이다.
# Stream Operator `operator <<`
사용자 정의 타입에 대한 입출력 작업을 수행하기 위해 스트림 연산자 `operator<<`을 오버로딩할 수 있다.
```cpp
#include <iostream>

struct Point {
	int x, y;

	friend std::ostream& operator<<(std::ostream& stream, const Point& point) {
		stream << "(" << point.x << ", " << point.y << ")";
		return stream
	}
};

Point point{1, 2};
std::cout << point; // print "(1, 2)"
```

# Binary Operators Note
Binary operators should be implemented as friend methods
```cpp
struct A{}; struct C {};

struct B : A {
	bool operator==(const A& x) { return true; }
};
struct D : C {
	friend bool operator==(const C& x, const C& y) { return true; } // inline
};
// bool operator==(const C& x, const C& y) { return true; } // out of line

A a; B b; C c; D d;
b == a; // ok
// a == b; // compile error // A는 ==연산자를 가지고 있지 않음
c == d; // ok, use operator==(const C&, const C&)
d == c; // ok, use operator==(const C&, const C&)
```
