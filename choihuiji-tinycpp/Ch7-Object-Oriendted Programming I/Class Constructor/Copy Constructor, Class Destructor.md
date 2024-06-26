# 복사 생성자 (Copy Constructor)
복사 생성자는 객체를 복사할 때 자동으로 호출되는 생성자로, 깊은 복사를 통해 새로운 객체를 생성하는 생성자이다.
```
#include <iostream>

struct A {
    A() { std::cout << "Default constructor called\n"; } // 기본 생성자
    A(int) { std::cout << "Non-default constructor called\n"; } // 비기본 생성자
    A(const A&) { std::cout << "Copy constructor called\n"; } // 복사 생성자
};

int main() {
    A a1; // 기본 생성자 호출
    A a2(10); // 비기본 생성자 호출
    A a3 = a1; // 복사 생성자 호출
    A a4(a2); // 복사 생성자 호출
    return 0;
}
```
### 특징
- 클래스는 항상 암시적 또는 명시적으로 복사 생성자를 정의
- 복사 생성자는 기본 클래스의 기본 생성자를 암시적으로 호출
- 복사 생성자가 명시적으로 정의된 경우, 이는 사용자 정의 생성자로 간주
- 복사 생성자는 템플릿 매개변수를 가질 수 없음
- 복사 생성자와 할당 연산자를 혼동하지 않도록 주의
```
struct MyStruct {};  
  
int main() {  
    MyStruct x;  // 기본 생성자 호출  
    MyStruct y{x}; // 복사 생성자 호출  
    y = x; // 할당 연산자 호출, 복사 생성자가 아니라 할당 연산자가 호출  
    return 0;  
}
```

```
struct Array {
    int size;      // 배열의 크기를 저장하는 변수
    int* array;    // 배열을 가리키는 포인터

    // 생성자
    Array(int size1) : size{size1} {
        array = new int[size];
    }

    // 복사 생성자
    Array(const Array& obj) : size{obj.size} {
        array = new int[size];
        for (int i = 0; i < size; i++)
            array[i] = obj.array[i];
    }
};

// 사용 예제
Array x{100}; // 크기 100인 배열을 가진 Array 객체 x 생성
Array y{x};   // x를 복사하여 새로운 Array 객체 y 생성, 복사 생성자 호출
```
### 사용 예시
- 같은 타입의 다른 객체로부터 한 객체를 초기화 할 때
```
A a1;
A a2(a1);  // 직접 복사 초기화
A a3{a1};  // 직접 복사 초기화
A a4 = a1;  // 복사 초기화
A a5 = {a1};  // 복사 리스트 초기화 (Copy List Initialization)
```
- 값으로 전달되는 함수 인자의 객체를 복사할 때
```
void f(A a);
```
- 함수의 반환 값으로 객체를 복사할 때
```
A f() { return A(3); }
```

```
#include <iostream>  
  
struct A {  
    A() {}  
    A(const A& obj) { std::cout << "copy" << std::endl; }  
};  
  
void f(A a) {} // pass by-value  
A g1(A& a) { return a; }  
A g2() { return A(); }  
  
int main() {  
    A a;  // 기본 생성자 호출  
    A b = a;  // 복사 생성자 호출 "copy"    A c(b);  // 복사 생성자 호출 "copy"    f(b);  // 복사 생성자 호출 "copy"    g1(a);  // 반환할 때 복사 생성자 호출 "copy"    A d = g2();  // RVO 최적화로 인해 복사 생성자 호출 생략 가능  
}
```
- RVO(Return Value Optimization)
	- 함수에서 객체를 반환할 때 복사 생성자 호출을 피하고, 직접 객체를 반환하도록 하는 최적화 기법
# 복사 생성자가 삭제된 경우
- 비정적 멤버 또는 기본 클래스가 참조 타입일 때
```
struct NonDefault { 
    int& x; 
}; 
// 참조 타입 멤버를 가지고 있으므로 암시적 복사 생성자는 삭제됨
```
- 클래스가 삭제되거나 접근 불가능한 복사 생성자를 가진 멤버 또는 기본 클래스를 가지고 있는 경우
```
struct NonDefault { 
    int& x; 
}; 

struct B { 
    NonDefault a; 
}; 
// B는 NonDefault의 인스턴스를 가지고 있으므로 암시적 복사 생성자는 삭제됨

struct C : NonDefault {}; 
// C는 NonDefault를 상속받으므로 암시적 복사 생성자는 삭제됨
```
- 클래스가 삭제되거나 접근 불가능한 소멸자를 가진 멤버 또는 기본 클래스를 가지고 있는 경우
- 클래스가 이동 생성자를 가질 때
# 소멸자
소멸자는 멤버 함수로, 객체가 범위를 벗어날 때나 'delete' 또는 'delete[]' 표현식이 해당 클래스의 포인터에 적용될 때 실행된다. 주요 목적은 리소스 해제 이며 문법은 클래스 이름 앞에 '~' 기호를 붙인 형태이다. 반환 타입이 없으며, 매개변수도 없다.
```
class T {
public:
    ~T() {
        // 자원 해제 코드
    }
};
```
각 객체는 하나의 소멸자를 가지며, 이는 암시적 또는 명시적으로 선언된다. C++20에서는 소멸자가 'constexpr'로 선언될 수 있다.
```
#include <iostream>  
  
struct Array {  
    int* array;  
    Array() { // 생성자  
        array = new int[10];  
        std::cout << "Constructor called\n";  
    }  
    ~Array() { // 소멸자  
        delete[] array;  
        std::cout << "Destructor called\n";  
    }  
};  
  
int main() {  
    Array a; // 생성자 호출  
    for (int i = 0; i < 5; i++) {  
        Array b; // 5번의 생성자와 소멸자 호출  
    } // "a"의 소멸자 호출  
}
```
C++에서 클래스 소멸자는 상속되지 않으며, 파생 클래스의 소멸자가 호출된 후 기본 클래스의 소멸자가 호출된다.
```
#include <iostream>  
using namespace std;  
  
struct A {  
    ~A() { cout << "A"; }  
};  
  
struct B {  
    ~B() { cout << "B"; }  
};  
  
struct C : A {  
    B b; // B의 인스턴스 선언  
    ~C() { cout << "C"; }  
};  
  
int main() {  
    C c; // C의 인스턴스 생성  
    return 0; // C의 인스턴스가 범위를 벗어나면서 소멸자 호출 // CBA 출력  
}
```
