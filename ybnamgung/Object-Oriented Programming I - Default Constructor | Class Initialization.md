# Object-Oriented Programming I - Default Constructor | Class Initialization

# Default Constructor

기본 생성자는 인자가 없는 생성자이며, 모든 클래스는 암시적, 명시적, 혹은 삭제된 기본 생성자를 가지고 있다.

```cpp
struct A {
		A() {} // 명시적 기본 생성자
		A(int) {} // 사용자 정의 생성자
};
```

```cpp
struct A {
		int x = 3; // 암시적 기본 생성자
};

A a{}; // 기본 생성자를 호출한다. 'A a'와 같다.
```

암시적 생성자는 `constexpr`이다.

```cpp
struct A {
		A() { cout << "A"; } // 기본 생성자
};

A a1; // 기본 생성자 호출
// A a2(); // 함수 선언으로 해석
A a3{}; // 기본 생성자 호출

A array[3]; // "AAA" 출력

A* ptr = new A[4]; // "AAAA" 출력
```

클래스의 암시적 기본 생성자는 다음 경우에선 삭제된 것으로 표시된다.

- 사용자 정의 생성자를 하나라도 가지고 있는 경우
- 래퍼런스 또는 const 형식의 비정적 멤버/기본 클래스를 갖고 있는 경우
- 삭제된(또는 접근할 수 없는) 기본 생성자를 가진 비정적 멤버/기본 클래스를 갖고 있는 경우
- 삭제되었거나 접근할 수 없는 소멸자를 가진 비정적 멤버/기본 클래스를 갖고 있는 경우

# Class Initialization

초기화 리스트는 클래스의 데이터 멤버를 초기화하거나, 생성자 본문에 들어가기 전에 명시적으로 기본 클래스의 생성자를 호출하는 데 사용된다. (std::initializer_list와 혼동하지 말 것)

```cpp
struct A {
		int x, y;
		
		A(int x1) : x(x1) {} ; // 초기화 리스트
		
		A(int x1, int y1) : // 초기화 리스트
				x{x1},
				y{y1} {}
};
```

C++11에서는 클래스 내 비정적 데이터 멤버 초기화(NSDMI)를 통해 데이터 멤버를 선언하는 곳에서 초기화할 수 있다. 사용자 정의 생성자를 사용하여 이러한 기본 값을 재정의할 수 있다.

```cpp
struct A {
		int x = 0;
		const char* str = nullptr;
		
		A() {} // 멤버 데이터는 초기화한 값에서 바뀌지 않는다.
		
		A(const char* str1) : str{str1} {} // str의 초기화 값이 바뀐다.
```

const와 래퍼런스 데이터 멤버는 초기화 리스트를 사용하거나 클래스 내 중괄호 또는 등호 초기화 구문(C++11)을 사용하여 초기화해야 한다.

```cpp
struct A {
		int x;
		const char y; // 초기화 필요
		int& z; // 초기화 필요
		
		int& v = x; // 등호 초기화
		const int w{4}; // 중괄호 초기화
		
		A() : x(3), y('), z(x) {}
```

클래스 멤버 초기화는 초기화 리스트의 순서가 아니라 선언된 순서를 따른다.

```cpp
struct ArrayWrapper {
		int* array;
		int size;
		
		ArrayWrapper(int user_size) :
				size{user_size},
				array{new int[size]} {}
				// 틀림! 'size'는 여전히 정의되지 않았다.
};

ArrayWrapper a(10);
cout << a.array[4]; // segmentation fault
```