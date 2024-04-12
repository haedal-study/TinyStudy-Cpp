#5 Basic Concepts IV - Memory Management

-technical term(?)
    - {}; brace ->      curly brackets,
    - [];               square brackets,
    - () parentheses -> round brackets,
    - <>                angle brackets,

## 5.1 **Heap and Stack**

   <img src="/images/MemoryStructure.png" width="320" height="180">

### 5.1.1 StackMemory
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
### 5.1.2 new,delete new[], delete
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

### 5.1.3 Non-Allocationg Placement
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

### 5.1.4 Non-Throwing Allocation:
    - 일반적으로, 메모리 할당이 실패하는경우에, 예외처리에러를 발생시킵니다.
    - 하지만 new (std::nothrow)를 사용하여 널포인터를 예외처리 에러가 발생하는 대신 사용할 수 있습니다.
    - **주의** 생성자에서는 여전히 예외 처리 에러를 발생시킨다는 것을 유의합니다. 

### 5.1.5 Memory Leak
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
## 5.2 Initialization

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
            -  Uniform Initialization (brace-initialization)
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
### 5.2.2 structure Initialization
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
###  5.2.3 Non-Static Data Member Initialization (NSDMI)
        - 비스태틱 를 맴버를 curly braces('{}')와 eqauls('=')를 사용하여 직접 정의하여 초기화 하는 방식입니다.
        ```
            struct S
                unsigned x = 3;
                unsigned y = 2;
            struct S1{
                unsigned x{3};
            };
        ```
### 5.2.2 Designated Initializer List
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
### 5.2.3 Structure Binding (구조체 바인딩)
        - C++17에서, 구조체,배열튜불등의 복합 데이터 타입의 요소를 변수에 직접 바인딩 하는 간편한 방법을 제공합니다.
        ```
        struct A {
        int x = 1;
        int y = 2;
        } a;

        auto [x1,y1] = a; // auto는 컴파일러가 자동으로 A타입으로 결정
        ```
### 5.2.4 Dynamic Memory Initialization(동적 메모리 초기화)
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

### 5.3 Pointers and Reference
    - What are Pointers?
        - 포인터는 (T*) 는 메모리의 주소를 나타냅니다
        - Pointer Dereferencing (역참조)
            - 포인터 역참조란, 포인터가 가리키는 배열의 주어진 위치에 있는 요소에 접근할 수 있도록 해줍니다. 
        - Subscript Operater[] (서브스크립트 연산자)
            - 서브스크립트 연산자는 'ptr[index]'로 사용되며, 포인터가 가리키는 배열의 위치(주소)의 값(요소)에 접근할 수 있도록 합니다
                - 이는 *(ptr + index)와 동일합니다
                - 즉 ptr[index]를 사용해서 인덱스의 요소에 접근할 수 있다는 뜻 입니다.
        -Pointer Operations
            - 포인터(e.g. void*) 의타입은 unsigned integer이며, 컴퓨터 아키텍쳐에 따라 32 혹은 64 비트로 결정됩니다.
            - 덧셈,뺄셈,증감 연산을 지원하여, 메모리간 위치이동을 쉽게 할 수 있습니다.
            - 추가로 동등, 관계연산자를 사용하여, 같은위치를 가리키는지, 메모리 주소예따라 순서를 정하는 등의 작업을 수행할 수 있습니다.
            - ```size_t y = (size_t) x; // (explicit conversion)``` 와 같이 명시적 변환으로 오프셋 계산에 활용할 수 있는 정수타입으로 변환할 수 있습니다. 암시적 변환은 지원되지 않습니다.      
        - pointer Conversion
            - 모든 포인터 타입은 모두 암시적으로 void*타입으로 변환 될 수 있습니다.
            - void 타입이 아닌경우(int*,char*같은 것들), 반드시 명시적으로 변환되어야 합니다. 
            - 예를들어 
            ```
            int* ptr5=;
            char* ptr6 = (char*) ptr5;// 이런경우 명시적 변환이 필요합니다 . 
            // 다만 static_cast로 수행할 수 없으며(static_cast<char*>(ptr5)), 타입안전성에 위배되기 떄문입니다. 
            ```
    - Address of operator `&`
        - address of operator ('&')는 , 메모리주소를 변수로서 저장하기위해 사용됩니다
        ```
        int a= 3;
        int* b = &a;
        a++;
        cout << *b // 4 출력
        ```
    - Wild & Dangling Pointer
        - wild pointer
        ```int main(){int* ptr;}```와 같이, 선언은 되었으나 확실한 주소로 초기화 되지않은 포인터를 말합니다.  
        - dangling pointer
        ```
        int* array = new int[10];
        delete[] array;
        delete[] array; // double free, or corruption에러가 발생합니다.  
        ```할당이 해제된 메모리 위치를 가리키는 포인터를 의미합니다, 예를들어 삭제된 배열을 접근하고있다면, 댕글링 포인터를 다루고 있는 것 입니다.

    - ### Reference
        - 참조의 안전성
            - 예를들어 int&는 정수형 변수에 대한 참조이며, 다시말해 다른 변수의 별명(alias)이라고도 생각할 수 있습니다. 또한 그 변수를 직접  가리킵니다.
            - 참조는 포인터보다 안전한 방식으로 여겨지는데, 이유는 다음과 같습니다
                - NULL 불가:  
                    - 참조는 NULL값을 가질 수 없기 때문입니다.
                    - 즉 참조가 항상 유효한 메모리에 연결되어 있다고 간주할 수 있습니다.
                - 불변성
                    - 즉 한번 객체에 대한 참조로 초기화 된다면, 다른 객체로도 변경될 수없습니다
                    - 반면 포인터는 가리키는 참조객체를 언제든지 변경할 수 있습니다.
                - 초기화 강제
                    - 생성시 반드시 초기화가 되어야합니다
                    - 반면 포인터는 그렇지 않습니다.
                    ```
                    int c = 2;
                    int& d = c;
                    int& d = 
                    ```
        - 참조 관련 문법 `T& var = ...`
            ```
            // int& a;     // 컴파일 에러, 반드시 초기화 되어야합니다.
            // int& b = 3; // compile error "3"은 변수가 아닙니다.   
            int c = 2;
            int& d = c;    // reference, ok vaild initialization 
            int& e = d;    // 참조의 참조또한 올바른 초기화 입니다. ok the reference of a reference is a reference. 
            ++d;
            ++e;
            cout << c      // 4가 출력됩니다.     
            ```
        - 참조와 포인터 인수에 관련한 차이
            ```
            void f(int* value) {}// value는 NULL 포인터가 될 수 있습니다
            void g(int& value) {}// value는 절대 NULL 포인터가 될 수 없습니다.

            int a = 3; 
            f(&a); // 
            f(0); // 위험하긴 하나 동작은 합니다, 다른 숫자로는 동작하지 않습니다
            f(nullptr)//f(0)과 같습니다
            //f(a) // 컴파일에러가 발생합니다, a는 포인터가 아니기 때문입니다.
            
            g(a);
            ```
        - 참조로 고정길이 배열을 가리킬 수도 있습니다
        ```
        void f(int (&array)[3]) { // size가 3이아니면 받을 수 없다!
        cout << sizeof(array);
        }
        
        int A[3], B[4];
        int* C = A;
        //------------------------------------------------------
        f(A); // ok..12출력
        // f(B); // compile error B has size 4
        // f(C); // compile error C is a pointer
        ```
    - 배열에서의 참조
        - 배열의 참조, 포인터 그리고 배열의 크기를 계산하는 여러가지 방법에 대한 예시
        ```
        int A[4];
        int (&B)[4] = A; // ok, reference to array , B는 A배열과 동일한 데이터를 가리킵니다.  
        int C[10][3];
        int (&D)[10][3] = C; // ok, reference to 2D array
        auto c = new int[3][4]; // type is int (*)[4]
        // read as "pointer to arrays of 4 int"

        // int (&d)[3][4] = c; // compile error, c는 배열에 대한 포인터이지만, d는 참조이기에 타입 불일치
        // int (*e)[3] = c; // compile error, c의 타입과 e타입(int(*)[4])은 서로 다릅니다 .

        int (*f)[4] = c; // ok
        int array[4];
        // &array is a pointer to an array of size 4
        int size1 = (&array)[1] - array; // 배열 다음주소에서 , 배열 시작주소를 뺌으로써, 배열의 크기를 계산
        int size2 = *(&array + 1) - array;
        cout << size1; // print 4
        cout << size2; // print 4
        see also www3.ntu.edu.sg/home
        ```
    -구조체 멤버 접근   
        - 지역변수와 참조로 구조체 멤버에 접근할땐 `dot(.)`을 사용합니다
        - 포인터에서 구조체로 접근할땐, arrow(->)를 사용합니다.
        ```
        Struct A{
            int x;
        };

        A a;
        a.x;

        A& ref = a;
        ref.x;
        A* ptr = &a;
        ptr ->x;
        ```


    - Constraints, Literals, const, constexpr, consteval,constinit**
        - Constant and Literals | const
        - constexpr
        - consteval, constinit, if constexpr
        - std::is_constant_evaluated() | if consteval
        - volatile keyword

    - Explicit Type Conversion
        -static_cast,const_cast, reinterpret_ cast
        -Type Punning
        -sizeof operater
    
## Constants and Literals | const
- 상수 
    - 한번 초기화 되었다면, 값이 변할 수 없는 변수를 말합니다.
    - 즉 런타임에 한번 초기화 되면 절대 바뀌지 않는 값이라고도 할 수 있습니다.
- 리터럴
    - 코드에 직접적으로 저장된 고정된 값이라고 할 수 있습니다.
    - 42,3.14,"Hello World" 이런값들을 리터럴이라고 부를 수 있습니다
    - 상수와 상응하는 면이 있으나, 힙이나 스택이 아닌 데이터 영역에 저장되며, 반드시 전역변수라는 특징이 있습니다. 
- const 
    - const라는 키워드를 사용해서 상수를 선언할 수 있습니다.
    - ``` const in maxScore = 100; ```
## constexpr (Constant Expression, Integral constant expression)
- C++ 11에서 새롭게 도입된 키워드로, 키워드나 객체,     함수 앞에 붙여 해당 객체나 함수의 리턴값을 컴파일 타임에 알 수 있다 라는 의미를 전달하게 됩니다.
- 컴파일 타임에 상수를 확실히 사용하고싶다면 constexpr 키워드를 꼭 사용해야 합니다. 
- constexpr vs const
    1. const로 정의된 상수는 굳이 컴파일 타임에 그 값을 알 필요는 없습니다.
    2. 하지만 constexpr 의 경우 반드시 오른쪽에 다른 상수식에 와야합니다. 다시말해, 반드시 컴파일 타임에 
    값을 명시해야합니다.
    3. constexpr의 경우 반드시, const이지만, 반대로 
    const의경우 반드시 constexpr이라고 할 수 없습니다.
    ```

    ```
## consteval | constinit | if constexpr
- consteval 
    - 함수가 반드시 컴파일 타임에 평가되도록 합니다. 만약에 컴파일타임에 평가가 이루어 질 수 없다면 컴파일 에러를 발생시킵니다
    - constexpr과 달리, 상수표현식 체크가 엄격합니다. 
    ```
    
    ```
- ConstInit
    - C++ 20 부터 지원하며, 변수가 컴파일 타임에 변수가 초기화 되는 것을 보장하는 역할을 합니다.
    - const constinit은 constexpr을 암시하지 않습니다. 반면, constexpr인경우는 const constinit을 보장합니다. 
- if constexpr 
    - C++17 이후 부터 지원하며, 컴파일 시간에 조건을 확인 할 수 있게 해줍니다.
    - 내의 조건은 상수 표현식이어야 하며, 조건이 참인 경에만 컴파일 됩니다. 
    -참고로 constexpr의 인수가 런타임에 돌아가도 함수자체는 동작합니다. (즉, 강제성 없음)
        - 코드예시
        ```
        constexpr int fuctionA(int x)
        {
        return x * x;
        }

        int main() {

        int a = 5;
        a= fuctionA(a);
        std::cout << a;

        }
            ```


## std::is_constant_evaluated() | if consteval

- std::is_constant_evaluated()
    - 컴파일타임에, 상수표현식으로 평가되는지 체크합니다
    - if constexpr과 다르게, 런타임과 컴파일 타임 모두 사용가능합니다.

- if consteval
    - if consteval을 사용함으로서, consteval, constexpr등이 가지고 있는 
    잠재적 코드오류를 해결할 수 있습니다.
    - 예시1
    ```
    // 함수f가 함수g를 실행하는 경우, 컴파일 타임에 실행되더라도 n 이 컴파일 타임 상수가 아니라면 undefined behavior에러가 발생합니다.
    consteval int g(int n) { return n * 3; }
    constexpr int f(int n) {
        if (std::is_constant_evaluated())
            return g(n); // This is problematic if n is not a compile-time constant.
        return 4;
}

    // 이렇게 if consteval을 활용하는 경우, n까지 검사해서 모두 constant 맥락인 경우에만 실행되므로 (아닌경우 컴파일에러) 안전합니다. 
    consteval int g(int n) { return n * 3; }
    constexpr int f(int n) {
        if consteval
            return g(n); // Always safe, executes only if the entire call chain is constexpr.
        return 4;
        }   
    ```


 ### volatile Keyword
    - 컴파일러 최적화를 방지하기 위한 type qualifer(타입한정자) 입니다.
    - 컴파일러에게, 변수의 값이 **프로그램의 다른부분** (하드웨어 등)에 의해 언제든지 변경될 수있다는 것을 알려줍니다. 
    - 사용예시
        - Hardware I/O Operation 
            - 어셈블리나 드라이버 개발 등에서 로우레벨 프로그래밍 시, 특정 명령하는 코드가 최적화 되어 실행이 안되버리는 상황을 방지할 수 있습니다.
        - inter-thread(multi-thread) communication
            - 스레드간 공유되는 자원인 경우, 스레드간 통신을 위하는경우
        - 그외에 최적화를 하고싶지 않은 경우에 사용가능합니다. 
    - volatile 키워드와 최적화 레벨 '-03'(전체 최적화)

    ```
    /*
     오히려 최적화를 방지해서, 의도하지않은 동작을 할 수 있습니다. volatile은 최적화 방지용이 아닌, 변수가 외부이벤트(하드웨어 인터럽트 등)에 변경 될 수 있는 경우에 사용되는 것이라고 할 수 있습니다.즉 밑의 코드같은 사용은 프로그래밍 오류라고 할수 있습니다.
     */
    volatile int* ptr = new int[i];
    int           pos = 128 * 1024 / sizeof(int);
    ptr[pst]          = 4; //세그먼트 오류 발생,,(오버플로우)
    ``` 

**Explicit Type Conversion**
- static_cast, const_cast, reintepret_cast

    - static_cast
        - 컴파일 타임에서 타입체크를 합니다. 
        - 안전하지않거나 우연하게 발생할 수 있는 타입간 변환을 방지할 수 있습니다
    - const_cast
        - 참조나 포인터가 가리키는 값의 상수성(constantness)를 제거합니다
        ```
        // 레거시 코드가 const를 올바르게 쓰지않은경우 const_cast를 사용할 수 있습니다. 
        const int c1 = 10;
        int* midfiable = const_cast<int*>(&c1);
        ```
    - _reinterpret_cast
        - 가장 위험한 방식의 캐스팅인데, 다양한 타입을 다룰 수 있게 해주는 만큼 디버깅이 어려워질 수 있기 때문입니다.
        - 사용예시
            ```
            // char -> float -> uintptr_t 타입으로 바꿔볼 수있습니다.
            char* bytes = new char[10];
            float* float_ptr = reinterpret_cast<float*>(bytes);
            *float_ptr = 3.14;
    
            std::uintprt_t address = reinterpret_cast<std::uintptr_t>(float_ptr); //메모리 주소값이 커도 충분히 표현됨
            ```
    - const_cast와 reinterpret_cast는 CPU 명령어로 컴파일 되지 않는데, 이는 이러한 캐스팅 방식이 런타임에 추가적인 요구를 하지않는다는 것을 의미합니다. 즉 실제로 메모리의 데이터를 변형시키거나, 변환하는게 아니라, 컴파일러가 코드를 해석하는 방식을 변경합니다. 


- Type Punning
- sizeof Operator
