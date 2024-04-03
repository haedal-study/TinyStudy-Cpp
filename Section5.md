#5 Basic Concepts IV - Memory Management

-technical term(?)
    - {}; brace ->      curly brackets,
    - [];               square brackets,
    - () parentheses -> round brackets,
    - <>                angle brackets,

##5.1 **Heap and Stack**

   <img src="/images/MemoryStructure.png" width="320" height="180">

   - stack과 heap의 차이를 요약하면 다음과같습니다
    - 정렬방식
         -둘다 인접(contigous)메모리 방식입니다.(메모리 공간을 순서대로 정렬하는 방식을 말합니다.)
    - 최대 사이즈  
         -스택은 상대적으로 적은 메모리를 사용합니다. 리눅스의 경우 8MB, 윈도우의 경우 1MB정도를 할당받습니다. 지역(임시)변수만을 할당받기 떄문입니다.
         - 시스템 메모리 만큼 할당가능하며, 크고 지속되는 정보를 할당하기 적합합니다.
    - 메모리 초과의 경우
        -  스택의 경우 시스템 크래쉬(스택 오버플로우)가 발생합니다. 기본적으로 초과하는 경우 지역변수할당과 리턴주소를 메모리 용량을 더 확보하지 않기 때문입니다.
        - 힙 메모리의 경우, 즉각적인 프로그램 크래쉬가 발생하지는 않지만, 메모리 할당은 실패 할 수 있습니다. 이런 경우  예외처리 오류나 널포인터를 반환합니다.
    - 메모리 할당
        - 스택의 경우 **컴파일 타임** 에 메모리 할당이 일어납니다. 즉 사이즈와 생명주기는 컴파일타임에 결정됩니다. 다만 런타임에 크기가 바뀜으로 힙과함께 동적할당영역이라고 부릅니다. 
        - 반면힙의 경우는 런타임에 할당됩니다.
    - Thread view
        - 스레드 관점에서, 모든 스레드는 각각 자신의 스택을 가집니다. 즉 각 스레드는 각각의 지역 변수들을 가집니다
        - 힙은 모든 스레드가 공유하는 영역입니다. 여러개의 스레드가 접근하는 경우, 적절한 동기화로 스레드끼리의 충돌을 피해야 합니다. 
        
    -멸망예시
        -아래 코드는 출력이 되긴합니다(컴파일 에러 안남) 하지만 Illegal memory access입니다!
        ```
        int* f() {
        int array[3] = {1, 2, 3};
        return array;
        }
        int* ptr = f();
        cout << ptr[0]; // Illegal memory access!! A
        ``` 

        ```
        //- 1. xyz가 범위를 벗어나면 메모리 해제가 발생합니다. 근데 이 함수에서 str은 xyz 배열을 가리키도록 합니다
        //- 2. str은 해제가 된 메모리를 가리키고 있습니다. 즉 Dangling Pointer상태입니다.
        //- 3 따라서 str은 Dangling Pointer이므로, 언제든지 프로그램의 다른 부분에 의해 덮어씌워질 수 있습니다. 

        void g(bool x) {
        const char* str = "abc";
        if (x) {
            char xyz[] = "xyz";
            str = xyz;
        }
        // If "x" is true, 'str' points to 'xyz' which goes out of scope here
        std::cout << str;
        }
        ```


- heap의 메모리할당 키워드
    - new/new[] , delete/delete[]는 동적 메모리 할당/해제를 나타내는 키워드입니다.
## 5.2 new,delete new[], delete
    - C언어에서 malloc과 free로 힙영역의 할당되 메모리를 해제하고 객체 생성, 메모리 블럭의 소멸등을 제어했습니다.
    - C++에서는 이를 new와 delete 키워드로 제어하게됩니다.
    - new 와 delete의 장점들
        - 언어 키워드 이므로 함수보다 안전합니다.
        - new는 정확한 데이터타입을 반환하는 반면, malloc()은 void*타입을 반환합니다.
        - 실패처리시, new는 예외를 발생시키나, malloc()은 널포인터를 반환합니다. 이는 무시할 수 없으며, 크기가 0인 할당에 대한 특별한 코드가 필요하지 않습니다.
        - 할당크기; new 키워드를 사용하면 컴파일러가 바이트 수를 계산해주지만, malloc을 사용하는 경우 사용자가 직접크기를 정해줘야합니다.
        - 다형성: 가상 함수를 가진 객체는 가상 테이블 포인터를 초기화 하기위해 new 로 할당되어야합니다
    - 동적 메모리할당 사용예시
    ```
    int* value = (int*) malloc(sizeof(int));
    int* value = new int;
    int* array = (int*) malloc(N * sizeof(int));
    int* array = new int[N]
    //N개 구조체 할당
    MyStruct* array = (MyStruct*)malloc(N*sizeof(MyStruct));
    MyStruct* array = new MyStruct[N]
    //N개 요소 할당 후 0으로 초기화
    int* array = (int*) calloc(N,sizeof(int));
    int* array = new int[N](); C++

        // 단일객체(원소) 메모리 해제
    int* value = (int*)malloc(sizeof(int));
    free(value);
    
    int* value = new int;
    delete value
    
    // N개 원소 해제
    int* value = (int*) malloc(sizeof(int));
    free(value);

    int* value = (int*)malloc(N*sizeof(int));
    free(value);

    int* value = new int[N];
    delete[] value;

    //배열
    int** A = new int*[3];
    for (int i=0; i<3; i++)
    A[i] = new int[4];

## 5.3 Non-Allocationg Placement
    - Non-Allocation Placement를 통해서, 메모리의 위치를 지정할 수 있습니다. 이는, 사용자가 직접적으로 메모리 사용을 관리하고 싶을때 매우 유용합니다.
    - 코드예시
        ```
        //Stack Memory
        // buffer array를 활용하여, 스택메모리의 공간을 할당 할 수 있습니다.
        char buffer[8];
        int* x = new(buffer) int;
        short* y = new(x+1) short[2];
        // no need to deallocatie x,y

        //Heap Memory
        //힙메모리에도 메모리를 할당할 수 있으나 사용된 메모리는 **반드시** 해제해주야 합니다.
        //이를 제대로 하지 않을때 메모리 누수가 발생합니다. 
        unsigned* buffer2 = new unsigned[2];
        double* z = new (buffer2) double;
        delete[] buffer2;
        //delete[] z; // ok, but bad practice

##5.4 Non-Throwing Allocation:
    - 일반적으로, 메모리 할당이 실패하는경우에, 예외처리에러를 발생시킵니다.
    - 하지만 new (std::nothrow)를 사용하여 널포인터를 예외처리 에러가 발생하는 대신 사용할 수 있습니다.
    - **주의** 생성자에서는 여전히 예외 처리 에러를 발생시킨다는 것을 유의합니다. 

## 5.5 Memory Leak
    - 힙영역에 동적으로 할당된 메모리가 프로그램에 사용되지않음에도 불구하고 실행시간동안 계속 남아잇는 것을 의미합니다.
    - 메모리 누수의 문제점 
        - illegal access로 인해 예기치 못한 동작이 발생합니다.
        - 정의되지 않은 값의 사용과 전이(전파), 세그멘테이션 오류, 잘못된 결과 발생가능
            - 변수를 초기화 하지 않고 사용하게 되면 불규칙 랜덤값이 발생하는 경우나, 잘못된 포인터의 사용으로 인해 배열의 범위를 넘어서는 접근등으로 발생할 수 있습니다. 
            - 참고:
                -Segmentation fault
                    - a segmentation fault (often shortened to segfault) or access violation is a fault, or failure condition, raised by hardware with memory protection, notifying an operating system (OS) the software has attempted to access a restricted area of memory (a memory access violation)
                    - Many programming languages have mechanisms designed to avoid segmentation faults and improve memory safety. For example, Rust employs an ownership-based[2] model to ensure memory safety.[3] Other languages, such as Lisp and Java, employ garbage collection,[4] which avoids certain classes of memory errors that could lead to segmentation faults.
        - 추가 메모리 소요 (잠재적 문제)
        - 참고로 복잡한 응용프로그램에서 더 발견하기 어려운 특징이 있습니다. 
    - 메모리 누수 예시
        ```
        int main(){
            int* array = new int[10];
            array = nullptr; //memory leak!
        }
        ```
## 5.6 Initialization
    - C++에서는 다양한 방식으로 변수를 초기화 할 수 있습니다
        - Variable Initialization
            - C++ 03에서는 기본초기화 (정의되지 않은 디폴트값), 직접 초기화, 복사 초기화 등 다양한 방법이 존재합니다.
            코드예시
            - 기본 초기화는 객체가 직접 초기값을 사용하여 생성됩니다. 복사초기화는 객체가 다른객로 부터 복사됩니다.
            ```
            //직접 초기화
            int a2(2); 
            int a3(0);

            // 복사초기화
            int a6 = 2; 
            int a4 = 2u 
            ```
        - Uniform Initialization (brace-initialization)
            - C++ 11 이후에 도입되었으며, 변수, 객체, 구조체등 여러 엔터티를 일관된 방식으로 초기화 할 수 있게 해줍니다.
            - 장점
                - 안전한 산술 연산 및 암시적 형변환에서 발생할 수 있는 값손실을 방지합니다.
                - 현대적인 형변환(캐스팅)보다 문법이 더간단합니다.
            ```
            //직접초기화
            int b1{2};
            int b2{};

            //복사초기화
            int b3 = int{}; //zero-initialization
            int b4 = int{4};
            int b5  = {};

            unsigned b6 = -1 
            unsigned b7{-1} // compile error

            float f2 = 10e40; // ok, "inf" value
            //float f3{10e40}; // compile error
            ```
            
        - Fixed-Size Array Initialization
            - 일차원 배열의 경우 명시적 또는 암시적(explicit or implicit size assignment) 크기 지정이 가능합니다.
            - 초기화 되지 않은 요소는 0으로 설정됩니다.
            - 이차원 배열의 경우는 각 차원의 크기를 명시하거나 일부 차원에 대해 암시적으로 크기를 결정할 수 있으나, 모든 차원의 크기를 생략하는 것은 허용되지 않습니다.
            -코드예시
            ```
            int a[3] = {1, 2, 3}; // explicit size
            int b[] = {1, 2, 3}; // implicit size
            char c[] = "abcd"; // implicit size
            int d[3] = {1, 2}; // d[2] = 0 -> zero/default value
            int e[4] = {0}; // all values are initialized to 0
            int f[3] = {}; // all values are initialized to 0 (C++11)
            int g[3] {}; // all values are initialized to 0 (C++11)

            int a[][2] = { {1,2}, {3,4}, {5,6} }; // ok
            int b[][2] = { 1, 2, 3, 4 }; // ok
            // the type of "a" and "b" is an array of type int[]
            // int c[][] = ...; // compile error
            // int d[2][] = ...; // compile error
            ```
### structure Initialization
    - 구조체와 동적메모리 초기화는 버전에 따라 다양한 방법으로 진행됩니다.
    - 코드예시 C++ 03
    ```
    struct S {
        unsigned x;
        unsigned y;
    };

    S s1;
    S s2 = {};
    S s3 = {1,2};
    S s4 = {1};
    S s5(3,5); //컴파일 에러

    S f() {
        S s6 = {1,2};//verbose (C++11보다 장황하다)
        return s6; 
    }
    ```

    - 코드예시 C++11
    ```
    struct S {
        unsigned x;
        unsigned y;
        void* ptr;
    };
    S a1;

    S f() { return { 3, 2 }; }// less(or non) verbose than C++03 structure initialization

    int main() {
    
        S a1{ 2,3 };
        a1 = f();
        std::cout << a1.x << "   " << a1.y;
    }
    ```
    - Non-Static Data Member Initialization (NSDMI)
        - 비스태틱 를 맴버를 curly braces('{}')와 eqauls('=')를 사용하여 직접 정의하여 초기화 하는 방식입니다.
        ```
            struct S
                unsigned x = 3;
                unsigned y = 2;
            struct S1{
                unsigned x{3};
            };
        ```
    - Designated Initializer List
        - C++20 부터 맴버를 직접 지정하여 초기화하는 방식을 지원합니다
        - 코드 가독성을 높이는데 도움이 됩니다.
        ```
        struct A{
            int x,y,z;
        };

        A a1{1,2,3};
        A a2{.x =1, .y =2, .z =3};
        ``` 

        void f1(bool a, bool b, bool c, bool d, bool e){}

        struct B {
            bool a, b, c, d, e;
        };

        struct B {
            bool a,b,c,d,e;
        };
        f2({.a=true, .b= true})
    - Structure Binding (구조체 바인딩)
        - C++17에서, 구조체,배열튜불등의 복합 데이터 타입의 요소를 변수에 직접 바인딩 하는 간편한 방법을 제공합니다.
        ```
        struct A {
        int x = 1;
        int y = 2;
        } a;

        auto [x1,y1] = a; // auto는 컴파일러가 자동으로 A타입으로 결정
        ```
    - Dynamic Memory Initialization(동적 메모리 초기화)
        - new 연산자를 사용하여, 힙 메모리에 동적(런타임)으로 메모리를 할당하고 초기화합니다
        - C++11부터 더 간결하게 초기화를 할 수 있는 기능이 추가되었습니다
        ```
        C++03:
        int* a1 = new int; // undefined
        int* a2 = new int(); // zero-initialization, call "= int()"
        int* a3 = new int(4); // allocate a single value equal to 4
        int* a4 = new int[4]; // allocate 4 elements with undefined values
        int* a5 = new int[4](); // allocate 4 elements zero-initialized, call "= int()"
        // int* a6 = new int[4](3); // not valid
       
        C++11:
        int* b1 = new int[4]{}; // allocate 4 elements zero-initialized, call "= int{}"
        int* b2 = new int[4]{1, 2}; // set first, second, zero-initialized
        ```


    
