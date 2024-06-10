
## **Polymorphism** 다형성

- **다형성이란**
	- 객체지향의 핵심중 하나이며, 한 객체가 **특정 사용 문맥**에 따라 다양한 형태를 가질 수 있음을 의미합니다.
	- 런타임에서, 기본 클래스의 객체는 파생 클래스의 객체처럼 동작합니다. 즉 런타임에서 객체의 실제 타입에 따라 다른 메서드를 호출할 수 있습니다.
	- Base 클래스에서 구현된 다형성을 가진 메서드는, 파생클래스에서 재정의 될 수 있으며, 이를 통해 각 파생 클래슨는 자신의 방식으로 메서드를 구현할 수 있습니다.
- **다형성 vs 오버로딩**
	- 오버로딩의 경우, 컴파일 시간에 결정 되는 정적 다형성 (static polymorphism)의 한 형태입니다.
	- 다만 C++에서는, PolyMorphism이 주로 동적 다형성에 연관되어 사용됩니다.
```C++
// 오버로딩 예시, 함수이름은 같고 받는 파라미터가 다릅니다. 
void f(int a){}
void f(double b){} 
```

- **함수 바인딩(Function Binding)**
	- 함수 호출과 함수 본문을 연결하는 것을 바인딩이라고 합니다
		- 주로, 변수나 함수 또는 객체와 관련된 특정 값이나 메모리 위치를 연결하는 과정을 말합니다. 
	- 바인딩의 종류에는 초기바인딩(정적 바인딩)과, 늦은바인딩(동적 바인딩) 있습니다
		1. 초기 혹은 **정적 바인딩**의 경우 (Early binding 혹은 Static Binding) .
			1. 컴파일타임에 함수호출을 결정하는 방식입니다.
			2. 컴파일러가 실행 시점에 객체의 유형을 식별하여, 프로그램이 직접 함수 주소로 이동할 수 있도록 합니다
		2. 늦은 혹은 **동적 바인딩** (Late Binding 혹은 run-time 바인딩)
			1. 런타임에 함수호출을 결정하는 방식입니다.
			2. C++ 에서는 가상함수(Virtual Method)를 선언하여 늦은 바인딩을 달성합니다. 

- **다형성 관련 문제** 
	- 기본 클레스와 파생클래스간의 메서드 호출시, 파생클래스의 메서드가 호출되지않고, 기본 클래스의 메서드만 호출되는 문제가 발생할 수 있습니다. 이를 해결하기위해 virtual 키워드를 사용하여 메서드를 가상 메서드로 선언하면, 런타임에 올바른 메서드를 호출할 수 있게됩니다. 
- ### Virtual Method 
	- 가상함수 키워드는 파생함수에서 필수는 아니지만, 가독성을 높이기때문에 쓰는 것이 권장됩니다. 또한 override 키워드 또한 쓰는것이 권장됩니다.
	```C++
	struct A {
	 virtual void f() override {cout << "A";} 
	}; 
	struct B : A{
		void of() {cout << "B";}
	}
	void g(A& a){a.f();}

	A a;
	B b;
	g(a);
	g(b);
// the case that virtual works
	struct A{
	 virtual void f() {cout << "A";}
	};
	
	struct B : A {
		 void f() { couit << "B";}
	};

	void f(A& a) {a.f();} // 타입의 참조를 매개변수로
	void f(A* a ){a->f(); } // 포인터를 매개변수로 
	void h(A a){a.f();}
	
	Bb;
	f(b); //print"B"
	f(&b); //print"B"
	h(b); // Print "A" struct B가 전달되더라도, struct A로 형변환되어 struct A의 f()가 호출됩니다. (객체 슬라이싱이 발생하여, A부분만 목사됩니다.)

```

### Virtual Table :  [링크..]([씹어먹는 C++ - <6 - 3. 가상함수와 상속에 관련한 잡다한 내용들> (modoocode.com)](https://modoocode.com/211))

- 가상테이블이란, 다형성 구현의 한가지 방법입니다.
- 함수 호출을 해결하고, 동적 바인딩(Dynamic Dispatch)를 지원하기위해 사용되는 **함수 조회 룩업테이블** 입니다. 
- 가상 테이블에는 클래스 객체가 호출할 수 있는 각 가상함수에 대한 항목이 하나씩 포함되어 있습니다. 이 테이블의 각 항목은 해당 클래스에서 접근할 수 있는 가장 파생된 함수로의 포인터 입니다.
- 이테이블을 가리키는 숨겨진 포인터를 기본 클래스에 추가합니다. 
- 관련 예제 (교재외에서 구함) 
```C++ 
struct A { virtual void f() { cout << "A::f" << endl; } };
struct B : A { void f() override { cout << "B::f" << endl; } };
int main() { A* obj = new B(); obj->f(); // "B::f" 출력 delete obj; return 0; }
```
![[Pasted image 20240524184510.png]]
- 가상함수가 아닌경우 이러한 과정을 거치지 않고 직접적으로 호출됩니다.
- 가상함수인 경우, **가상 함수 테이블을 한 단계 더 거쳐**서 실제로 어떤 함수를 고를지 결정하게 됩니다. 
- ````
```C++
struct A{
virtual void f();
virtual void g();
};

struct B : A{
 void f();
}

int main(){
A a = B();
c -> func1();
}
```
### 가상함수는 정말 존재하는가?
- malloc을 쓰면 vtable이 생성되지않아 에러가나는 예제 코드입니다.
- C++에서는 malloc을 절대 쓰지않는게 권장됩니다.
```C++
struct A{
int x = 3;
virtual void f(){cout << "abc";}
};

A* a1 = new A;
A* a2 = (A*) malloc(sizeof(A));

cout << a1 ->x;
cout << a2 ->x;
a1 -> f();
a2 -> f();
```


- 가상함수는 추가적인 포인터를 할당하며 이는 가려져있습니다
	- 아래 예제에서 A는 가상함수로 인해 두개의 vtable 포인터를 포함하게 됩니다. (이에 따라 크기가 증가합니)
``` C++
struct A {
	virtual void f1();
	virtual void f2();
};

class B : A{};

int main() {
	
	cout << sizeof(A); //8(64bit) 출력 // 만약 virtual 아닌경우 1출력
	cout << sizeof(B);
}

```


