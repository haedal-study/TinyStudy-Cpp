# 4.Basic Concepts III - Entities and Control Flow

### 4.1Entities 
- C++ 에서 사용되는 값,객체, 참조, 함수, 열거형, 타입, 클래스 멤버, 템플릿 또는 네임스페이스를 의미합니다.
- 부연설명 
    - 템플릿(Template) 
        - 타입 매개변수를 가지는 함수나 클래스를 정의하여 일반화 프로그래밍(제네릭 프로그래밍을) 가능하게 합니다.
        - C# 에서 제네릭 과의 차이점
            - 둘다 타입재사용성과 런타임에서의 타입안정성을 성능저하없이 구현가능합니다.
            - C++은 컴파일 타임에 해당 타입에 대한 새로운 코드가 생성되는 방식이나, C#은 런타임에 타입이 결정됩니다. 
            - C#은 또한 CLR에서 타입마다 별도로 인스턴스를 생성하는 것이 아닌, 같은 IL(Intermediate Langugage)를 사용합니다.
            - 이러한 차이로 C++은 프로그램 크기가 커질 수 있는 방식임에 반해, 런타임 성능이 매우 우수합니다.
            - 반대로 C+에서는 프로그램크기가 상대적으로 작지만, 컴파일 타임의 최적화는 C++에 비해 덜합니다. 
            - C++은 상대적으로 오류메세지가 상대적으로 길고 복잡하여 디버깅이 어려울때가 있습니다.  
        
### 4.2 Declaration and Definition
- Declaration
    - 함수나 변수 같은 엔터티들을 컴파일에게 미리 알려주는 것을 선언이라고 합니다.
    - 엔터티는 여러번 선언될 수 있습니다.
-Definition
        - 정의는 선언에 대한 구현이며, 엔티티의 속성과 정의를 정의합니다. 
        - 각각의 엔터티에 대해 한번의 정의만 허용됩니다.
        ```
        void f(int a, char* ) // 매개변수명을 알필요 없이 타입만 알려줘도 충분합니다. 
        {
            // definition..
        } 

        void g(); 
        g(); // 정의가 없으니 Linking Error가 발생합니다

        struct A;
        struct A; // 중복 선언가능
        struct B // 정의 
        {
            int b;
            A x; // 컴파일 에러; 불완전타입이기 때문입니다.
            A* y // 하지만 포인터는 불완전 타입을 가리킬 수 있습니다.
        }

        struct A { //정의
            char c;
        }
        ```
### 4.3 Enumerators 
    - 열거형(enum)과 강타입 열거형(enum class)
    - 기본열거형(enum)
        - 서로 관련있는 값을 그룹으로 묶어 명명된 상수를 정의하는데 사용되는 타입을 말합니다.
        - 주로 가독성을 높이고, 오류감소, 코드명확도 증가를 위해 사용합니다. 
        -C 언어에서부터 제공되어왔습니다.
        - 문제점
            - 다른 enum타입과 혼동될 수 있는 문제 
            - 예를들어 color와 fruit이라는 enum 타입이 있는경우, color와 fruit변수를 비교하면 둘이 같다고 판단될 수 있습니다
            이를 해결해기 위해 enum class 가 도입되었습니다.
    - 강타입 열거형
        - 강력한 타입체크를 제공하여, 다른 열거타입의 값이나 정수값이로 암묵적 형변환이 일어나지 않습니다. 
        - 기본 타입을 설정할 수 있어 더 효율적으로 메모리를 사용할 수 있습니다.
    - 강타입 열거형의 버전별 개선된 점
        - C++ 17
            - 초기화 목록을 사용해 enum class를 초기화 할 수 있습니다.
            - 속성 지정을 통해 컴파일러에게추가정보를 제공할 수 있습니다.
        - C++ 20
            -스위치문에서, enum class 이름없이 직접 맴버이름을 사용할 수 있게 되었습니다.
            - 스위치문 안에서 using enum EnumName을 선언합니다. 
    - 흔한 오류
        - enum, enum class를 항상초기화해야합니다.
        - 타입 범위 밖으로 캐스팅을하면 정의되지 않은 행동을 유발시킬 수 있습니다. 
    -enum 특징
        - enum/ enum class는 서로 비교연산 가능합니다.
        ```
        enum class Colr {RED, GREEN, BLUE};
        cout << (Color::RED < Color::GREEN);// print true
        ```

        - enum/ enum class는 자동적으로 오름차순으로 증가합니다. 
        ```
        enum class Color {RED, GREEN = -1,BLUE, BLACK};
        cout << (Color::RED <Color::GREEN); // print true
        ```
        - 여러 이름을 동일한 값에 할당가능합니다
        ```
        enum class Device{PC =0, COMPUTER=0, PRINTER};
        ```
        - C++ 11 부터 enum class는 기본타입으로 int8_t같은 정수타입을 사용할 수 있어, 메모리사용을 최적화 할수 있습니다.
            - enum/enum class를 사용할 때 기본 정수타입은 구현에 따라 다를 수 있으나 기본적으로 int로 설정됩니다. 즉 32비트 크기를 가지기 때문에 때로 필요이상으로 큰 메모리 공간을 차지 할 수 있습니다.
        - C++ 17부터 배열처럼 초기화 가능합니다
        ```
        enum class Color{RED,GREEN,BLUE}
        Color a{2} 
        ```
        -C++17 부터 enum/enum class 에서 속성(attribute)를 사용하여 컴파일러에게 추가정보를 제공합니다. 
        ```
        enum class Color {RED, GREEN, BLUE[[deprecated]]};
        auto x = Color::BLUE; //Compiler Warning
        ```
### 4.3 struct, Bitfield and union
- struct
    -개요
        - 다양한 타입의 변수를 하나의 단위로 묶는데 사용됩니다.
    -특징
        - 구조체 정의다음 바로 struct 변수를 한개이상 생성할 수 있습니다
        ```
        struct A{int x;} a,b;
        ```
        - 열거형 또한 구조체안에 **익명으로** 선언 가능합니다
        ```
        struct A{enum{X,Y}}; 
        A::X;
        ```
        - 함수내부, 지역 범위에 구조체를 선언할 수 있습니다.
        - int f(){
            struct A{
                int x;
            } a;
            return a.x;
        }
    
-Bitfield
    - 구조체 변수내에 특정 비트폭을 미리정의하는 방식입니다.
    - 필요한 비트 수 만큼만 데이터를 사용하여 데이터를 압축할 수 있습니다.
    - 비트필드를 사용하면 자동으로 모듈로 연산 효과를 낼 수 있습니다.
    -예시
        ```
            struct S1{
                int b1 : 10; //범위 [0,1023]
                int b2 : 10;
                int b3 :  8; //범위 [0,255]
            }
            
            struct S2{
                int b1 : 10;
                int : 0; //새로운 32bit 경계를 맞추기위해 reset합니다. 
                int b2;
            }//8byte
            // b1은 10비트를 사용하고, b2도 10비트를 사용합니다
            // 다만 int :0 으로 인해 32bit경계가 맞춰져있습니다. 
        ```

    - 참고 :
        - 기본 자료형보다 비트 범위가 크면 컴파일 에러가 납니다

    ```
    	struct S4 {
		int b1 : 1500;  // 컴파일에러
		int b2 : 10;
		char c2 : 4; 참고로 혼용할경우 32 + 4bit처럼 계산되고, 결과는 8바이트가 됩니다.
	};
	std::cout << sizeof(S4);

    ```
        - 최소 메모리는, 비트필드가 선언된 기본타입에 따라 결정됩니다 
        ```
        	struct S4 {
		char a : 3;
		char b : 1;
		char C : 1;
        };
        std::cout << sizeof(S4); // 1 출력
        ```

- union
    개요
        - 다른 데이터 타입들을 **같은 메모리공간(위치)** 에 저장할 수 있게 해주는 특별한 데이터 타입 입니다.
        - 공용체의 크기는 가장 큰 데이터 멤버를 저장할 수 있을 만큼만 큽니다.
        -C++ 17 에서는 std::variant를 도입하여 타입안전한 union을 나타낼 수 있습니다.
    - 장점 및 사용처
        - 메모리의 효율적사용
        - Type Punning (형변환없이 데이터 사용하는 것)
        - 하드웨어 인터페이스
    - 주의사항
        - 데이터가 **공유**되는 특성으로 인해 버그가 발생할 여지가 있다는것을 염두해야합니다.
        - 즉 Union을 사용할때는 한번에 한타입만 유효하다는 것을 기억하고 사용해야합니다.



### 4.4 - if statement | for and while Loops | Range-based for Loop
    - if 조건문
        - 조건이 참이면 코드블록을 실행합니다.
        - Short-circuiting (단락평가)
            - 단락(단축)평가란, 결과가 결정되는 순간 평가가 멈추는 것을 의미합니다
            - 예를들어 if(<true expression> || array[-1] ==0) 인경우 에러는 발생하지않습니다
            - 삼항연산자
                -   <condition> ? <expression1:try> : <expression2>
                - 중첩사용 예시
                ``` int value = (a == b) ? a : (b == c ? b : 3); // nested``` 
    - for와 if
        - for 얼마나 반복될지 **알 때** 사용
        - while 얼마나 반복될지 **모를 때** 사용
        - do while
            - while과 똑같으나 기본적으로 한번은 사용합니다.
        특징
            - for에서는 루프에서 여러 변수를 정의할 수 있습니다.
            - 무한루프 
                for(;;)를 사용하여 무한히 실행되는 루프를 만들 수 있으며, while(true)와 같습니다.
        - 점프문 
            - break: 즉시 루프를 종료합니다
            - continue 현재 반복의 나머지 부분을 건너뛰고 새로운 반복을 시작합니다.
            - return : 루프뿐이 아닌 전체 함수를 종료합니다.  
    - Range-based for 루프
        - C++11에서 도입된, 더 읽기 쉽고 안전한 요소 순회 방법입니다.
        - 루프변수를 수동으로 관리하고 증가시킵니다.
        - 특징
            - 고정 큭기배열, 초기화리스트, begin(), end() 메서드를 가진 메소드에 사용할 수 있습니다.
        - 예시
        ```
        int values[]= {3, 2, 1};
        for (int v: values)
        cout << v <<  ""; // 3 2 1 을 프린트합니다. 

         
        for (values[]= {3, 2, 1})
        cout << v <<  ""; // 3 2 1 을 프린트합니다. 

        for(auto c: "abcd")
        cout << c << "";
        ```
        - 구조바인딩과의 혼용
            - C++17에서 ranged-based loop을 구조바인딩 개념까지 확장시켰습니다. 이 기능은 구조체나, 튜플을 개별 변수로 풀어내는 것을 가능하게 합니다. 위에서 말한 범위기반 for 루프와 함께 사용하면 복잡한 객체들을 담고 있는 컨테이너 요소에 쉽게 접근할 수 있습니다

            ```
            std::array<std::pair<int, int>, 3> array = {{{1,2}, {5,6}, {7,1}}};
            for (auto [x, y] : array) {
            cout << x << ", " << y << "; ";
}
            ```

            ```
            struct A {
            int x;
            int y;
            };

            A array[] = { {1,2}, {5,6}, {7,1} };
            for (auto [x1, y1] : array)
            cout << x1 << "," << y1 << " "; // print: 1,2 5,6 7,1
            ```

- ### 4.5 switch | goto | Avoid Unused Variable Warning 
    - switch
        - switch는 블럭을 돌며 특정 조건에 맞는 블럭을 실행할 수 있게 합니다
          ```
           
            int x = 2;
            switch (x) {
            case 0:  x; // nearest scope
            case 1: std::cout << x; // undefined!!
            case 2: { int y=3; std::cout << y; } // ok
            }
          
          ```
        - Fallthrough
            - C와 마찬가지고 C++에서 Case 블록이 끝나도 break 문이 없으면 다음 case 블록으로 이어집니다. 이를 fall-through  현상이라고 합니다.
            - C++ 17 이후 부터는 [[fallthrough]]속성을 사용하여 의도적인 fall-through를 명시 할 수 있습니다
            ```
            char x =... 
            int y =0;
            switch (x){
                case 'a' x++;
                    [[fallthrough]] // C++ 17 : avoid warning + notify intentional fallthrough
                case 'b': return 0;
                default: return -1;
            }
            ```

    - goto
        - 다른 파트의 프로그램 실행부분으로 점프 할 수 있습니다
        - 근데 대부분의 경우 유지보수와 코드가독성이 크게 떨어지므로 사용을 지양합니다. 
            - 스파게티 코드의 원흉이되기도 합니다
        - 최신 C++에서는 조건문과 return을 적절히 사용하여, 루프혹은 함수를 조기종료 합니다.
    
    - Avoid Unused Variable Warning [[baybe_unused]]
        - C++ 17이후부터 (왠지~) 사용안할 것같으면 '[[maybe_unused]]라는 키워드를 사용해서 컴파일러 경고를 방지할 수 있습니다.특정 컴파일러만 지원하는 것이 아닌 standard-compliant C++ 컴파일러라면 전부 지원합니다.(in a portable way) 
        ```
        int foo(int value){
            [[maybe_unused]] int x = value;
        #if defined (ENABLE_SQUARE_PATH)
            return x * x;
        #else
        // static_cast<void>(x) //before C++ 17
            return 0;
        #endif
        }
        ```