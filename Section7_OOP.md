### 7.1.1 구조체와 클래스

- 구조체와 클래스는 **의미적으론 동일**하지만, 다음과같은 차이점이 있습니다.
	- 구조체는 수동적인 객체, 즉 물리적인 상태 혹은 데이터 집합을 의미합니다. 
	- 반면 클래스는 능동적인 객체를 말하며, 논리적인 상태를 말합니다. 데이터 추상화를 나타낸다고 표현하기도 합니다.


참고: https://banaba.tistory.com/34
### 7.1.2 RAII 

- Resource Acquisition is Initalization 의 약자이며, 리소스관리를 단순하고 안전하게 하기위한 설계패턴을 의미합니다.
- 리소스의 생성(획득 및 초기화), 사용, 해제 단계로 이루어집니다. 
	- **리소스 생성** (리소스를 클래스에 캡슐화 하기)
		- 리소스(메모리,파일 핸들, 소켓 등)를 객체의 생성자에서 획득하고 초기화합니다.
		- 예를들어 파일을 열거나 메모리를 할당하는 작업은 객체의 생성자에서 수행됩니다.
		
	- **리소스 사용** (로컬 인스턴스 사용하기)
		- 리소스는 객체의 메서드를 통해 사용되며, 객체가 살아있는동안 안전하게 리소스를 사용할 수 있습니다.
		- 
	- **리소스 해제** (객체가 범위를 벗어나면 자동으로 리소스 해제하기)
		- 객체가 더 이상 필요없어서 범위를 벗어나면, 객체의 **소멸자**가 자동으로 호출됩니다.
		- 소멸자는 리소스를 해제하는데 사용되는데, 파일을 닫거나 메모리를 해제하는 작업은 소멸자에서 수행됩니다.
		- C++ 은 가바지 컬렉터가 없으며,  프로그래머가 리소스를 관리할 책임을 가집니다. 이는 추후 메모리누수 개념과 밀접한 관련이 있습니다.


### 7.1.3 Class Hierachy

    1. Child/Derived Class or Subclass
        새로운 클래스가 다른 클래스의 함수와 변수를 상속받으면, 파생혹은 자식클래스라고 부릅니다.
    2. Parent/Base Class
        위의 파생 혹은 자식클래스에게 함수와 변수를 제공하는 **가장 가까운** 클래스를 부모 또는 기본클래스라고 합니다
        cf. 가장가깝지 않은경우, 예를들어 (A<-B<-C) 상속구조인 경우 A는 C의 조상클래스(Ancestor Class)라고 부릅니다. 

``` C++
        struct A{
            int value =3;
            vold g() {}
        };

        struct B : A{
            int data = 4;
            int f(){return data;}
        };


        b.g(); // A의 멤버함수
```
    1. C++의 구조체와 클래스에서 상속 및 함수호출시 복사와 참조에 관한 다양한 코드예제
```C++ 
    struct A {};
    struct B : A{};
    void f(A a) {}
    void g(B b) {}
    void f_ref(A& a){}
    void g_ref(B& b){}

    A a;
    B b;

    f(a); // f는 타입 A를 받음
    f(b); // B는 A를 상속받았고, B 타입 객체를 A타입으로 복사하여 호출

    f_ref(a);
    g_ref(b);

    //g(a) : g는 A타입을 받을 수 없기때문에 컴파일 에러가 발생합니다.
    // g_ref(a) : g_ref는 A타입 참조를 받을 수 없습니다.
    g(b);

    A a1 = b; // B는 A를 상속받았기 때문에, B타입 객체를 A타입으로 복사하여 초기화가 가능합니다.
    A& a2 = b;// 이 또한 타입참조 바인딩(참조변수를 다른 변수에 연결하는 것)이 가능합니다 
    // B b1 = a; // 하지만 반대로,A 타입 객체 a를 타입 객체로 복사하는 것은 불가능 합니다.   

    int main() {
    B b; 
    b.value = 10;

    A a1 = b; // B는 A를 상속받았기 때문에, B타입 객체를 A타입으로 복사하여 초기화가 가능합니다.
    A& a2 = b;// 타입참조 바인딩(참조변수를 다른 변수에 연결)
    std::cout <<"current Value" << a2.value << std::endl; //10
    b.value = 15;
    std::cout <<"Modified Value" << a2.value << std::endl; //15 
}
```

### 7.1.3 Access Specifier 접근한정자

- 접근 지정자는 상속된 맴버의 가시성을 정의합니다.
	- 이는 데이터은닉 개념과 관련이 있습니다.
- 접근지정자 종류: public: 제한없음, protected : 함수맴버와 파생클래스에서 접근가능, private 함수멤버만 접근가능
- struct는 기본적으로 public 맴버이며, class는 기본적으로 private 맴버입니다. 
- 참고(배경)
	- struct는 데이터 그룹화가 기본 용도였으며, 기본적으로 모두 공개여야 데이터를쉽게 접근하고 사용할 수 있었기 떄문에 기본적으로 public이 되었습니다.
	- 반면, class는 객체지향 프로그래밍을 지원하기 위한 개념이었으며, 데이터 은닉과 캡슐화를 통해, 객체의 내부상태를 보호하고, 접근을 제어하기 위해 기본적으로 private  설정이 되어있습니다. 


### 7.1.4 Default Constructor 

- 디폴트생성자는 매개변수를 받지않는 생성자를 말하며, 객체가 아무런 초기화 없이 생성될때 호출됩니다.
	- 명시적으로 정의하지 않으면 컴파일러가 자동호출합니다.
	- 객체를 기본상태로 초기화합니다. 
- 반면 Non-디폴트 생성자는 매개변수를 받는 생성자이며, 디폴트생성자와 달리 반드시 명시적으로 호출합니다. 
- 생성자는 클래스의 특별한 멤버함수이며, 해당 클래스의 새로운 인스턴스가 생성될때 실행됩니다.
- 생성자의 주요 목표는 초기화와 리소스 획득입니다.
- 구문은 클래스 이름과 동일하며 반환형이 없게 작성합니다.

- 예시 1
```C++
    struct A{
        A() {} //명시적(explicit) 기본 생성자
        A(int) {} // 사용자 정의 생성자

        struct A {
            int x = 3; //묵시적(implicit) 기본 생성자
        }
        A a{} //ok
    }

     struct A {
        A() { cout <<"A";}
     }
```
    - 예시 2 
```C++
        struct A {
        A() {}
        A(int) { std::cout << "A"; }
    };


    int main() {
        A a1(3); //A출력
        A a3{}; // 직접리스트 초기화 (C++11 이후, direct-list 초기화)
        std::vector<int> vec = {1,2,3,4};
        A array[3]; // 출력X
        A* ptr = new A[4];// 출력X
    }
    ```

### 7.1.4 Deleted Default Constructor 
- 다음과 같은경우, 기본생성자는 삭제된것으로 간주됩니다.
    - 사용자 정의 생성자가 있는경우
```C++
    struct A {
        A(int x){}
    };

    A a; //컴파일에러
```
    - 비정적 맴버, 기반 클래스 참조, 혹은 const형타입인경우
```C++
    int a = 0;
    int& ref_a = a;
    const int b = 4;

        struct NoDefault
    {
    int& x;
    const int y;
    NoDefault(int& ref, int val) : x(ref), y(val) {}
    };

    int main() {
        // 기본생성자가 삭제된걸로 취급되기때문에 위와같이 클래스멤버 변수를 적절히 만들어 초기화 하여 컴파일해보았습니다.
        NoDefault a(ref_a,b);
    }
        

```
    
	- 비정적맴버 혹은 기반클래스가 삭제된 경우거나, 접근할 수 없는 소멸자를 가진경우
```C++
        struct A {
    private:
        ~A() {}

    
        static A* create() {
            return new A();
        }

        
        static void destroy(A* obj) {
            delete obj;
        }

    
        void say_hello() {
            std::cout << "Hello from A!" << std::endl;
        }
    };

    int main() {
        // 밑의 함수는 접근이 불가능합니다.
        // 접근자를 public으로 해서 힙 영역 메모리를 삭제하는 방식으로 바꿔야합니다.
        A* a = A::create(); 
        a->say_hello(); 
        A::destroy(a); 
        return 0;
    }
```
### 7.1.4 Initializer List
- 초기화목록이란, 클래스의 데이터맴버를 초기화하거나, 생성자 본문에 들어가기전에, 명시적으로 클래스 생성자를 호출하는데 사용됩니다.
- 추후, std::initializer_list와 혼동하지 않도록 합니다.

### 7.1.5 클래스 맴버 초기화자(NSDMI)
 - In-class non-static data members initialization, 클래스 내 비정적 데이터 맴버 초기화자를 C++11이후부터 도입하여, 데이터 맴버 선언 시, 초기화를 할 수 있게됩니다.
 - 이를 통해 사용자 정의 생성자로, 다른값으로 초기화 할 수 있습니다. 
 ```C++
 struct A {
    int x = 0;               // 클래스 멤버 initializer
    const char* str = nullptr; // 클래스 멤버 initializer

    A() {} // 기본 생성자가 호출되면 "x"는 0, "str"은 nullptr로 초기화됩니다.

    A(const char* str1) : str{str1} {} // "str"을 str1으로 초기화, "x"는 여전히 0
};
 ```
- const,참조형 데이터멤버는 반드시 초기화 목록이나, 멤버 초기화자를 이용해 초기화되어야 합니다.
```C++
struct A {
    int x;
    const char y;  // 반드시 초기화되어야 함
    int& z;       // 반드시 초기화되어야 함
    int& v = x;   // equal-initializer (C++11)
    const int w{4}; // brace initializer (C++11)

    // 초기화 목록을 사용하여 초기화
    A() : x(3), y('a'), z(x) {}
};
```


 ### 7.1.7 초기화 순서 
- 클래스 맴버의 초기화는 선언된 순서를 따르며, 초기화 목록의 순서가 아닌, 멤버변수가 선언된 순서대로 초기화합니다. 즉 아래코드는 컴파일에러는 발생하지 않지만, segmentation fault에러가 발생하게됩니다.
 ```C++
 struct ArrayWrapper {
    int* array;
    int size;

    ArrayWrapper(int user_size) :
        size{user_size},   // 'size'를 먼저 초기화
        array{new int[size]} // 그 다음 'array'를 'size'를 이용해 초기화
    {}
};
 ```

### 7.1.8 Uniform Initialization for Objects

 - C++11 이후에 도입되었으며, 중괄호'{}'를 사용하여, 어떤 데이터 타입의 객체인지 상관없이, 초기화 할 수 있는 방법을 말합니다. 이는 일관된 초기화 문법을 제공하며, 특정문제 (중복된 타입 이름 최소화, Most Vexing Parse) 를 해결하는데 도움이 됩니다. 

 - 중복 타입 이름 최소화 예제 코드
 ```C++
     
struct Point
{
    int x, y;
    Point(int x1, int y1) : x(x1), y(y1){}
};
//C++11
Point add(Point a, Point b) {
    return {a.x + b.x, a.y + b.y };
}
//C++3
 Pointadd(Pointa,Pointb){
 returnPoint(a.x+b.x,a.y+b.y);
 }
 Pointc=add(Point(1,2),Point(3,4));

int main() {
  
    Point c = add(Point(1, 2), Point(3, 4));
    std::cout<<c.x <<" "<< c.y;
}
 ```
### Most Vexing Parse
```C++
- 예제 1
        
    ```C++
        struct A {
        A(int) {}
    };

    struct B {
        A A(1); // 컴파일에러 발생
    };

    // 아래 코드와 같이 작성하여 컴파일 에러를 해결 할 수 있습니다.
    struct A{
        A(int) {}
    };
    struct B{
        A a; 
        B() : A(1) {}
    };

    //2
    struct A {
    A(int) {}
    };

    struct B {
        A a{1}; // 중괄호를 사용하여 a를 1로 초기화
    };

    // 혹은 다음과 같이
    A a{1}; // a를 1로 초기화

    ```
## **7.1.8 Constructors and Inheritance**
 - 클래스의 생성자는 절대 상속되지않습니다.
 - 즉 파생클래스는 생성자를 명시적,암시적으로 호출해야하며, 
 - 상위 기본클래스 -> 가장최신(가까운) 파생 클래스 순서로 호출됩니다.
```C++

```C++
struct A {
    A() { std::cout << "A"; }
};

struct B1 : A {
    int y = 3; 
};

struct B2 : B1 {
    // B2클래스의 생성자에서, B1 클래스의 생성자를 명시적으로 호출하는 구문 입니다. 
    B2() : B1() { std::cout << "B"; }
};

int main() {
    B1 b1; // "A" 출력
    B2 b2; // "A" 출력 후 "B" 출력

}
```

  ### **7.1.9 Delegate Constructor (대리생성자)**

  - 대부분 생성자에서는 개별작업 수행 전, 동일한 초기화 단계를 수행합니다.
  - C++11 에서는 동일 클래스의 다른 생성자 호출시, 반복 코드를 줄이기 위한 목적으로 대리생성자를 도입했습니다 
  ```C++
struct A {
    int a;
    float b;
    bool c;

    // 표준 생성자
    A(int a1, float b1, bool c1) : a(a1), b(b1), c(c1) {
        // 많은 작업 수행
    }

    // 대리 생성자
    A(int a1, float b1) : A(a1, b1, false) {}
    A(float b1) : A(100, b1, false) {}
};

int main() {
  
    A a(3.0f);
    std::cout << a.a << " " << a.b <<" "<< a.c; // 100 3 0 출력
}
  ```

  -  ### **7.1.10 **explicit 키워드**
    - 생성자나 변환 연산자가 단일 인자 또는 중괄호 초기화자로 부터, 암시적 변환이나 복사초기화를 허용하지 않음을 명시합니다.다음과같은 초기화 방식끼리 암시적으로 변환하며 혼란을 유발하지 않도록 하기위한 의도가 담겨있다고 할 수 있습니다.
       용어 정리
        - 직접 초기화(Direct Initialization):
             중괄호 {} 또는 소괄호 ()를 사용하여 객체를 직접 초기화하는 방식입니다.
        예: B b1{2};, B b2(2);
        - 복사 초기화(Copy Initialization):
        등호(=)를 사용하여 객체를 초기화하는 방식입니다.
        예: B b1 = 2; (단, explicit 생성자가 있으면 암시적 변환이 금지됨)

        - 값 초기화(Value Initialization):
        중괄호 {}를 사용하여 객체를 기본 값으로 초기화하는 방식입니다.
        예: B b2{};
        
        - 리스트 초기화(List Initialization):
        중괄호 {}를 사용하여 객체를 초기화하는 방식의 총칭입니다.
        예: B b1{2};, B b2{};
	
	
```C++
	struct MyString {
	MyString(int n);          // (1) n 바이트 할당
	MyString(const char* p);  // (2) 원시 문자열로부터 초기화
    };

    MyString string = 'a'; // (1) 호출, 암시적 변환!!

    // explicit 키워드 사용
    struct B {
        explicit B() {}
        explicit B(int) {}
        explicit B(int, int) {}
    };
    void f(const B&) {}
    int main() {
    
        // B b1 = {};   // 오류: 암시적 변환
        B b2(2);       // ok
        // B b3 = 1;    // 오류: 암시적 변환
        B b4{ 4, 5 };    // ok: B(int, int) 호출
        // B b5 = {4, 5}; // 오류: 암시적 변환
        B b6 = (B)1;   // ok: 명시적 캐스트
        // f({});      // 오류: 암시적 변환
        // f(1);       // 오류: 암시적 변환
        //f({1});     // 오류: 암시적 변환
        f(B{ 1 });       // ok
    }
```
        
- **7.1.11 **[nondiscard]
```C++

    C++17, 함수가 반환하는 타입의 결과를 무시하지 않게 컴파일에러를 발생시킵니다. 
    C++20에서는 생성자에도 이속성을 넣을 수 있도록 확장하였습니다. 
   
    ```C++
      struct A {
    [[nodiscard]] A() {} // 생성자에 적용
    };

    void f(A) {}

    int main() {
    A a{};     // ok: 반환 값을 사용함
    f(A{});    // ok: 반환 값을 사용함
    A{};       // 경고: 생성된 객체를 무시함
    return 0;
}
```

- ### **7.1.10  Copy Constructor
- 
- T(const T&)의 형태로서, 기존 객체의 새로운 깊은 복사본(deep copy)을 만드는 방식입니다. 
	-  깊은 복사를 하여, 객체가 각각 독립적으로 동작할 수 있도록 합니다. 
- 하나를 생성하고, 나머지를 '복사하여' 생성하는 방식이라고 할 수 있습니다.
- 다음과 같은경우에 복사생성자가 사용됩니다.
	- 1. 직접 생성자 호출
	- 2. 대입 연산자 사용
	- 3. 복사 리스트 초기화 
	- 4. 함수에서 입력 매개변수로 객체가 전달될때 (pass by value)
	- 5.함수에서 객체가 반환 될때 (RVO 최적화)
		- RVO (Return Value Optimization) 란 불필요한 복사를 제거하는 최적화 기법을 말합니다. 
			- RVO가 적용되면 복사 생성자나 이동 생성자가 호출되지 않습니다.
			- [C++ RVO, NRVO에 대해서 알아보자. - dydtjr1128's Blog](https://dydtjr1128.github.io/cpp/2019/08/10/Cpp-RVO(Return-Value-Optimization).html)

```C++

```C++
struct A {
	A() {}   // 디폴트 생성자
	A(int) {} // 논디폴트 생성자
	A(const A&) {} // 복사 생성
}
```

- 복사생성자 사용예시
``` C++
stuct Array{
int size;
int* arrray;

Array(int size1) : size{size1}
{
 array = new int[size]
}

Array(const Array& obj) : size{obj.size}{
array = new int[size];
for(int i=0; i < size; i++)
array[i] = obj.array[i]
}
```
```
```

- Pass By-Value and Copy Construct
	
	cheap copy 저렴한 복사
		- 자식 클래스에서 복사 생성자 호출하는경우, 상대적으로 더 간단한 작업을 수행합니다.
	 Expensive copy: 비싼 복사
		 - 부모 클래스에서 복사 생성자 호출 시, 더 많은 자원이나 시간을 소모하는 작업을 수행합니다. 
		 
```C++ 
struct A {
    A() {}
    A(const A& obj) {
        std::cout << "expensive copy\n";
    }
};

struct B : A {
    B() {}
    B(const B& obj) {
        std::cout << "cheap copy\n";
    }
};

void f1(B b) {}
void f2(A a) {}

// 심심해서 해본 시간 측정 코드.. 평균적으로 10마이크로 정도 cheap copy가 더 빠르다
int main() {
    B b1;
    const int iterations = 3000;
    double total_cheap_time = 0;
    double total_expensive_time = 0;

    // cheap copy 시간 측정
    for (int i = 0; i < iterations; ++i) {
        auto start_cheap_copy = std::chrono::high_resolution_clock::now();
        f1(b1); // cheap copy
        auto end_cheap_copy = std::chrono::high_resolution_clock::now();
        std::chrono::duration<double> duration_cheap_copy = end_cheap_copy - start_cheap_copy;
        total_cheap_time += duration_cheap_copy.count();
    }
std::cout << "Average cheap copy time: " << (total_cheap_time / iterations) * 1e6 << " microseconds\n";

    // expensive copy 시간 측정
for (int i = 0; i < iterations; ++i) {
auto start_expensive_copy = std::chrono::high_resolution_clock::now();
f2(b1); // expensive copy
auto end_expensive_copy = std::chrono::high_resolution_clock::now();
std::chrono::duration<double> duration_expensive_copy = end_expensive_copy - start_expensive_copy;
total_expensive_time += duration_expensive_copy.count();
    }
std::cout << "Average expensive copy time: " << (total_expensive_time / iterations) * 1e6 << " microseconds\n";

    return 0;
}

```
예제 2
``` C++
struct CustomArray {
	int size;
	int* array;

	CustomArray(int size1) :size{ size1 } {
		array = new int[size];
	}
	// copyconstructor, ":size{obj.size}" initializerlist
	CustomArray(const CustomArray& obj) :size{ obj.size } {
		std::cout << "copy constructor";
		array = new int[size];
		for (int i = 0; i < size; i++)
			array[i] = obj.array[i];
	}
};
int main()
{
	CustomArray x{ 100 };
	CustomArray y{ x }; 
}
```

### Class Destructor
- 소멸자
	- Destructor (~T())는 클래스의 객체가 소멸될 때 호출되는 특별한 멤버함수를 일컫습니다.
	- 1. 객체가 범위를 벗어나거나, 2. delete 또는 delete[] 표현식이 해당 클래스의 포인터에 적용될 때 실행됩니다. 
	- 구문 및 특징
		- ~T()의  이름은 클래스의 이름과 같으며 반환 타입은 없습니다.
		-  객체당 정확히 한개의 소멸자만 가집니다. 이는 명시적으로 선언 혹은 암식적으로 생성됩니다.
		- C++ 이후부터, constexpr로 선언될 수 있습니다.
	- ```
	- ```
``` C++
struct Array{
int * array;
Array(){
array =new int[10];
}

~Array()
{
std::cout << "delete!";
delete[] array;
}

	int main()
	{
		Array a;
		//생성자와 디스트럭터 각각 5번 호출
		for (int i = 0; i< 5; i++){
		Array b; 
	}
	}
```
- 클래스 소멸자에서 호출순서
	-  소멸자는 절대 상속되지 않습니다.
	- 상속 구조가 있을때, 가장 파생된 클래스부터 기반 클래스 순서로 호출됩니다.
```C++
struct A {
    ~A() { std::cout << "A"; }
};

struct B {
    ~B() { std::cout << "B"; }
};

struct C : A {
    B b; 
    ~C() { std::cout << "C"; }
};

int main() {
    C c; // 출력: "C", "B", "A"
}

```
### Class Keywards

- ### default
	- C++11 부터, 컴파일러가 기본 생성자, 소멸자, 복사/이동 생성자, 대입 연산자, spaceship 연산자를 자동 생성할 수 있도록 `default` 구문을 제공합니다.
	- 개발자가 함수를 명시적으로 정의하지 않고도, 필요한 경우에만 컴파일러가 자동으로 하기위함입니다.
		- SpaceShipt연산자의 경우 `auto operator <=> (const A&) const = default`로 두객체간 비교를 수행합니다.
	- 컴파일러 생성 멤버 함수의 유용성은 다음과 같습니다.
		1. 비밀성변경
		2. 멤버함수 가시성 변경
		3. 멤버 변수 기본값 강제
	```C++
	struct A 
	{ A(int v1) {} // 사용자 정의 생성자로 기본 생성자를 무시 
	A() = default; // 이제 A는 기본 생성자를 가지고 있음
	 };
	 
	 struct B 
	 { 
	 protected: B() = default; // 이제 B의 기본 생성자는 protected
	  };
	 struct C 
	 { int x; // C() {} 
	 // 'x'가 정의되지 않음 
	 C() = default; // 'x'는 0으로 초기화됨
	  };
	``````
```
- ###  this
	- 자기 자신을 가리키는 내부포인터 입니다.
	- 1.멤버 변수의 이름과 같은 로컬 변수 이름이 있을때, 2. 혹은 객체 자신을 참조하기위해 사용할 수 있습니다

```C++
struct A { int x; void f(int x) { this->x = x; // "this" 없이 x = x;는 아무런 효과가 없음 } 
const A& g() { return *this; // 객체 자신을 반환 } };
```
- static
	- 클래스 자체에 속하고, 모든 클래스 인스턴스가 공유하는 멤버를 선언하기 위해 사용됩니다.
	- 인스턴스 바운드가 아닌 멤버를 선언하는 것이라고도 표현할 수 있습니다
	- 클래스 이름으로 직접 호출하거나, 클래스 인스턴스를 통해 호출합니다.
- const
	- 데이터 멤버나 함수를 런타임에 변경할수 없음을 나타냅니다.
- mutable
	- mutable은 const 클래스 인스턴스의 데이터멤버를 수정할 수 있게합니다.

```C++
// const A로 선언하였으나, A.y는 mutable이므로 런타임에서 변경가능
struct A { int x = 3; mutable int y = 5; }; const A a; a.y = 5; 
```
- using
	- 타입 별칭을 선언하는데 사용됩니다.
	- 특정 클래스 이름 공간 내에서 다른이름으로 타입을 참조할 수 있게합니다.
	- 상속에서 using 키워드는 기존 클래스의 맴버를 새로운 클래스에서 재사용 할 수 있게합니다.

```C++
struct A { using type = int; };
struct B : A {};
typename B::type x = 3; // "typename" 키워드 필요
```

```C++
struct A {
protected:
    int x = 3;
};
struct B :A {
public:
    using A::x;
};


int main() {
    B b;
    b.x = 3; 
}

```
- friend
	- 특정 클래스나 함수에게 다른 클래스의 private 및 protect 맴버에 접근할 수 있는 권한을 줍니다.
	- friend 관계는 비대칭적입니다.
	- 상속에 의해 자동으로 확장되지 않습니다.
``` C++
class B; //classdeclaration

class A{
friend class B; //B 에게 A의 멤버인 x(private)에게 접근할 수 있는권한을 줍니다.
int x; //private
};
class B {
	int f(A a){ return a.x; } //ok,B is friend ofA
};
class C :B {
	// int f(A a){return a.x;}// compileerror notinherited
};
```

- delete
	- 특정 멤버 함수를 삭제하고, 해당 함수 사용시 컴파일 오류를 발생시킵니다.
	- 이는 복사/이동 생성자 또는 대입연산자에 적용합니다.
	- 안전하지 않거나 비용이 많이드는 경우를 방지하기위해 사용합니다.
	
```C++
struct A { A() = default;
A(const A&) = delete; // 복사 생성자가 삭제됨 };```
```