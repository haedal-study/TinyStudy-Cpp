C++에서 캐스팅의 종류는 아래와 같습니다.
- Upcasting
	- 파생 클래스의 참조 혹은, 포인터를 기본클래스로 변환하는 것을 말합니다.
	- 업캐스팅은 기본적으로 안전합니다.
	- `static_cast` 또는 `dynamic_cast`를 사용합니다.
- Downcasting
	- 기본 클래스의 참조 또는 포인터를 파생클래스로 변환합니다.
	- 명시적(explicit) 으로만 사용가능합니다.
	- 위험할 수 있습니다.
	- `static_cast` or `dynamic_cast` 사요요합니다.
- Sidecasting (Cross-cast)
	- 같은 계층 수준의 클래스간의 참조나 포인터를 변환하는 것을 말하빈다.
	- 명시적(explicit)으로만 사용 가능합니다.
	- 위험할 수 있습니다.
	- `dynamic_cast`를 사용합니다

예제코드
```C++
struct A {
 virtual void f() {cout<<"A";}
};

struct B : A{
int var =3;
void f() override{cout << "B";}
}

A a;
B b; 
A& a1 = b; 

static_cast<A&>(b).f();// 업캐스팅
static_cast<B&>(a).f();// 다운캐스팅
cout <<b.var;
cout <<static_cast<&B>(a).var; // potential segfault 발생! // 다운캐스팅


//사이드 캐스팅 예제코드
struct A { virtual void f() { cout << "A"; } };
struct B1 : A { void f() override { cout << "B1"; } };
struct B2 : A { void f() override { cout << "B2"; } };
B1 b1;
B2 b2;
dynamic_cast<B2&>(b1).f(); // sidecasting, std::bad_cast 예외 발생 (안전한 캐스팅을 보장하기 위함)

```
### Run-time Type Identification 

- RTTI, Run-time Type Information 은 객체의 타입을 런타임에 결정할 수 있게 해주는 메커니즘을 말합니다
- C++ RTTI는 다음 세가지 기능을 통해 표현됩니다.
	- `dynamic_cast`키워드: 다형성 타입 변환을 수행합니다.
	- `typeid` 키워드: 객체의 정확한 타입을 식별합니다.
	- `type_info` 클래스: `type` 연산자가 반환하는 타입 정보를 포함합니다.
- `typeid` 예시코드```
```C++
	struct A {
	virtual void f(){}
	} ;
	
	struct B : A{}
	
	A a;
	B b;
	A& a1 =b;
	cout<< typeid(a).name(); // 교재에선 1A라고 출력되나, vs2022에서 structA로 출
	cout<< typeid(b).name();
	cout << typeid(a1).name();
```
참고) 네임맹글링(name mangling)으로 인해 , 컴파일러는 함수 시그니처를 포함하여 유일한 이름을 생성합니다. 타입이름도 마찬가지로 컴파일러가 내부적으로 구분하기 위해, 추가적인 정보를 붙입니다.+

- `Dynamic_cast` 예시
	- `static_cast`와 다르게, RTTI를 사용하여, 변환하려는 타입의(output) 정확성을 검사합니다. 이 작업은 런타임에 수행되고, 비용이 많이 들어갑니다. 
	- 아래와 같은 속성들을 가집니다.
		- 파생클래스 객체를 기본 클래스 포인터나 참조로 변환합니다 (업캐스팅)
		- New/Obj가 참조형일 경우, 변환이 불가능 하면 `std::bad_cast`예외를 던집니다.
		- New/Obj가 포인터형일 경우, 변환이 불가능 하면 nullptr을 반환합니다. 
- `dynamic_cast`예시 코드
	```C++ 
	struct A { 
	    virtual void f() { cout << "A"; } 
	}; 
	struct B : A { 
	    void f() override { cout << "B"; } 
	}; 
	
	A a; 
	B b; 
	
	dynamic_cast<A&>(b).f(); // "B" 출력, 업캐스팅
	// dynamic_cast<B&>(a).f(); // std::bad_cast 예외 발생, 잘못된 다운캐스팅
	dynamic_cast<B*>(&a); // nullptr 반환, 잘못된 다운캐스팅


// 예시코드 2 - get_object 함수로 A또는 B타입의 객체를 반환하고 , g함수가 이를 dynammic_cast를 통해 B타입으로 다운캐스팅을 시도합니다  
	struct A { 
	    virtual void f() { cout << "A"; } 
	}; 
	struct B : A { 
	    void f() override { cout << "B"; } 
	}; 
	
	A* get_object(bool selectA) { 
	    return (selectA) ? new A() : new B(); 
	} 
	
	void g(bool value) { 
	    A* a = get_object(value); 
	    B* b = dynamic_cast<B*>(a); // 다운캐스팅 및 검사
	
	    if (b != nullptr) // 포인터형이므로, 변환이 불가능하면 nullptr를 반환함, 이경우 실행하지 않습니다. 
	        b->f(); // 안전할 때만 실행
	}

	```
