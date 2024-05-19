### 7.1.1 구조체와 클래스
    - 구조체와 클래스는 의미적으론 동일하지만 다음과같은 차이점이 있습니다.
        - 구조체는 수동적인 객체, 즉 물리적인 상태 혹은 데이터 집합을 의미합니다. 
        - 반면 클래스는 능동적인 객체를 말하며, 논리적인 상태를 말합니다. 데이터 추상화를 나타낸다고 표현하기도 합니다.


참고: https://banaba.tistory.com/34
### 7.1.2 RAII 
    - Resource Acquisition is Initalization 의 약자이며, 리소스관리를 단순하고 안전하게 하기위한 설계패턴을 의미합니다.
    - 리소스의 생성(획득 및 초기화), 사용, 해제 단계로 이루어집니다. 
        -리소스 생성 (리소스를 클래스에 캡슐화 하기)
            - 리소스(메모리,파일 핸들, 소켓 등)를 객체의 생성자에서 획득하고 초기화합니다.
            - 예를들어 파일을 열거나 메모리를 할당하는 작업은 객체의 생성자에서 수행됩니다.
        - 리소스 사용 (로컬 인스턴스 사용하기)
            - 리소스는 객체의 메서드를 통해 사용되며, 객체가 살아있는동안 안전하게 리소스를 사용할 수 있습니다.
        - 리소스 해제 (객체가 범위를 벗어나면 자동으로 리소스 해제하기)
            - 객체가 더 이상 필요없어서 범위를 벗어나면, 객체의 **소멸자**가 자동으로 호출됩니다.
            - 소멸자는 리소스를 해제하는데 사용되는데, 파일을 닫거나 메모리를 해제하는 작업은 소멸자에서 수행됩니다.
        - C++ 은 가바지 컬렉터가 없으며,  프로그래머가 리소스를 관리할 책임을 가집니다. 이는 추후 메모리누수 개념과 밀접한 관련이 있습니다.


### 7.1.3 Class Hierachy
    1. Child/Derived Class or Subclass
        새로운 클래스가 다른 클래스의 함수와 변수를 상속받으면, 파생혹은 자식클래스라고 부릅니다.
    2. Parent/Base Class
        위의 파생 혹은 자식클래스에게 함수와 변수를 제공하는 **가장 가까운** 클래스를 부모 또는 기본클래스라고 합니다
        cf. 가장가깝지 않은경우, 예를들어 (A<-B<-C) 상속구조인 경우 A는 C의 조상클래스(Ancestor Class)라고 부릅니다. 

        ```
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
    3. C++의 구조체와 클래스에서 상속 및 함수호출시 복사와 참조에 관한 다양한 코드예제
    ```
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
    - 접근 지정자는 상속된 맴버의 가시성을 정의합니다. 데이터은닉 개념과 관련이 있습니다.
        - public: 제한없음, protected : 함수맴버와 파생클래스에서 접근가능, private 함수멤버만 접근가능
    - struct는 기본적으로 public 맴버이며, class는 기본적으로 private 맴버입니다. 
        - 참고(배경)
            - struct는 데이터 그룹화가 기본 용도였으며, 기본적으로 모두 공개여야 데이터를쉽게 접근하고 사용할 수 있었기 떄문에 기본적으로 public이 되었습니다.
            - 반면, class는 객체지향 프로그래밍을 지원하기 위한 개념이었으며, 데이터 은닉과 캡슐화를 통해, 객체의 내부상태를 보호하고, 접근을 제어하기 위해 기본적으로 private  설정이 되어있습니다. 


### 7.1.4 Default Constructor 

    - 생성자는 클래스의 특별한 멤버함수이며, 해당 클래스의 새로운 인스턴스가 생성될때 실행됩니다.
    - 생성자의 주요 목표는 초기화와 리소스 획득입니다.
    - 구문은 클래스 이름과 동일하며 반환형이 없게 작성합니다.
    - 예시 1
    ```
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
        ```
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
    ```
    struct A {
        A(int x){}
    };

    A a; //컴파일에러
    ```
    - 비정적 맴버, 기반 클래스 참조, 혹은 const형타입인경우
    ```
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
    ```
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
 ```
 struct A {
    int x = 0;               // 클래스 멤버 initializer
    const char* str = nullptr; // 클래스 멤버 initializer

    A() {} // 기본 생성자가 호출되면 "x"는 0, "str"은 nullptr로 초기화됩니다.

    A(const char* str1) : str{str1} {} // "str"을 str1으로 초기화, "x"는 여전히 0
};
 ```
- const,참조형 데이터멤버는 반드시 초기화 목록이나, 멤버 초기화자를 이용해 초기화되어야 합니다.
```
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
 클래스 맴버의 초기화는 선언된 순서를 따르며, 초기화 목록의 순서가 아닌, 멤버변수가 선언된 순서대로 초기화합니다.
 ```
 struct ArrayWrapper {
    int* array;
    int size;

    ArrayWrapper(int user_size) :
        size{user_size},   // 'size'를 먼저 초기화
        array{new int[size]} // 그 다음 'array'를 'size'를 이용해 초기화
    {}
};
 ```
