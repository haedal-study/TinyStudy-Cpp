### 6.1 Functions 

### 6.1.1 Pass by-Value | Pass by-Pointer |Pass by-Reference

- 참고
    -Function Parameter (Formal Parameter)
        - 함수 내부에서의 변수를 의미합니다.
    -Function Argument (Actual Parameter)
        - 함수에 전달되는 실제값(actual value)를 나타냅니다. 
    -함수 시그니쳐 (Function Signature) 어떤 함수를 정의할 때 ,해당 함수가 어떤 인자를 받고 어떤 타입의 값을 반환하는지 명시하는 부분입니다. 이 시그니처를 통해 컴파일러는 함수 호출이 올바른지, 즉 올바른 개수와 타입의 인자가 전달 되었는지 확인할 수 있습니다. 
        예를들어
        ```
        int add(int a, int b);
        ```                   
        여기서 함수의 시그니처는 int타입의 두 매개 변수를 받아들이고 int 타입의 값을 반환한다는 것을 나타냅니다.  
-  Pass-by-Value
             
    - 함수 호출 시, 인자는 함수 파라미터로 복제됩니다.
    - 함수 내부에서 파라미터는, 함수 외부의 변수와 분리됩니다.
    - 즉 파라미터에서의 값 수정은 원래 값에 영향을 주지 않습니다.
    ```
    void f(int x){
        x =10; // x라는 변수가 들어와도 x값은 바뀌지않습니다.
    }
    ```

-  Pass by Pointer(Call By Pointer)
    - 파라미터가 포인터인경우, 메모리 주소값이 전달되게 됩니다.
    - **역참조 함으로써** 함수가 전달된 인자(argument)를 수정할 수 있게됩니다.
    - 값의 복사가 이루어지지 않기 때문에 빠르나, 포인터가 null이거나, 잘못된 방식으로 값이 수정될 위이 있습니다.
    ```
    void f(int* x){
        if( x!= nullptr){
            *x =10; // 원본인자 변경
        }
    }
    ```

- 6참조에 의한 전달(Call by Reference)
    - 인자에 대한 참조가 함수로 전달되며, 실제 인자에 대한 별명(alias)이 됩니다.
    - 함수가 인자를 직접 변경할 수 있게 해주며, 복사를 하지 않습니다.
    - 포인터에 의한 전달만큼 빠르고,참조가 가진특성인  null이 될 수 없는 성질 때문에, 다른곳을 가리킬 수 없어 더욱 안전합니다.
    ```
    void f(int& x){
        x =10; //원본인자 직접변경
    }
    ```
- 코드예시
```
#include <iostream>
#pragma STDC FENV_ACCESS ON
struct MyStruct;

void f1(int a) {}; // pass by-value // 바디가 없으면 컴파일 에러가 발생함
void f2(int& a) {}; // pass by-reference
void f3(const int& a) {}; // pass by-const reference
void f4(MyStruct& a) {}; // pass by-reference
void f5(int* a) {}; // pass by-pointer
void f6(const int* a) {
	
}; // pass by-const pointer
void f7(MyStruct* a); // pass by-pointer
void f8(int*& a); // pass a pointer by-referencestruct S 
char c = 'a';



int main() {

	f1(c); // ok, pass by-value (implicit conversion)
	// f2(c); // compile error different types
	f3(c); // ok, pass by-value (implicit conversion) 
}
```



## 6.1.2 Function Signature and Overloading | Overloading and =delete
    - Function Signature
        - Function Signature는 함수의 인터페이스를 정의한다고 말할 수 있습니다.
        - 이는 인수의 수(갯수), 그 타입 그리고 전달되는 순서를 포함합니다.
        - 템플릿함수(아직 정확한 정의 모름)입력 및 출력의 타입도 추가로 포함합니다.
        - C++ 표준은 함수를 반환 타입으로만 구별할 수 없도록 규정하고, 인수의 타입이나, 순서가 달라야만 서로 다른것으로 간주합니다.

    - Overloading 
        - Overloading Resolution Rules
            - 제공된 인수에 기반하여, 어떤 오버로드 버전의 함수를 호출할지 결정하기 위해 아래와 같은 규칙을 사용합니다.
            - 첫번째 부터 높은 우선순위로 동작하는 점에 유의합니다.  
            - Exact match
                인수의 타입과 정확히 일치해야합니다.
            - Promotion
                정확한 일치가 없는경우, 컴파일러는 인수를 promotion하여, 매개변수 타입과 일치시킬 수 있는 함수를 찾습니다.
            - Standard Type Conversion
                - float에서 int로의 변환처럼, 프로모션이 적용될 수 없는경우, 다음과같이 표준변환을 사용하여 매개변수와 일치할 수 있도록 하는지 검사합니다.
            _ Constructor or User-defined Type Conversion
                - 위의 모든 규칙이 적요되지않을때, 생성자에 의한 변환 또는 타입 변환 연산자를 오버로딩하여 정의된 사용자정의 변환이 고려됩니다.
            ```
            void f(int a);
            void f(float b); // 오버로드
            void f(float b, char c); // 오버로드

            f(0); // 정확한 일치: f(int)를 호출
            // f('a'); // 컴파일 오류: 'a'는 int와 float로 프로모션 가능하여 모호함
            f(2.3f); // 정확한 일치: f(float)를 호출
            // f(2.3); // 컴파일 오류: 2.3은 double이며 int와 float로 변환 가능하여 모호함
            f(2.3, 'a'); // 표준 타입 변환을 통한 정확한 일치: f(float, char)를 호출
            ```
            - 오버로딩 예시
            ```
            void f(int a, char* b); // signature: (int, char*)
            void f(char* a, int b); // 이렇게 순서만 달라도 오버로딩이 됩니다!
            // char f(int a, char* b); // compile error same signature
            // but different return types
            void f(const int a, char* b); // same signature, ok
            // const int == int
            void f(int a, const char* b); // overloading with signature: (int, const char*)
            int f(float); // overloading with signature: (float)
            // the return type is differen
            ```
            - =delete를 사용한 오버로드 방지
            C++ 11이후 기능인 =delete 지정자는, 명시적으로 오버로드된 함수를 명시적으로 비활성화합니다.
            ```
            void g(int) {}
            void g(double) = delete; // 이 오버로드는 삭제됩니다
            g(3); // OK: g(int)를 호출
            g(3.0); // 컴파일 오류: g(double)삭제

            #include <cstddef> // std::nullptr_t 포함
            void f(int*) {}
            void f(std::nullptr_t) = delete; // nullptr를 가지고 f를 호출하는 것을 방지합니다
            f(nullptr); // 컴파일 오류: f(std::nullptr_t)가 삭제됩니다.
            ```
            - 예를들어 간단하게 이런식으로 주석처리만해도 동작하는것을 볼 수있는데, 이는 의도치않게 오버로딩되는 상황을 막을 수 있을것(?)
            ```
            void g(int) {}
            //void g(double) = delete; // 이 오버로드는 삭제됨.
            #include <cstddef> // std::nullptr_t 포함
            void f(int*) {}
            //void f(std::nullptr_t) = delete; // nullptr를 가지고 f를 호출하는 것을 방지함.
            struct A {};

            int main() {
                g(3); // OK: g(int)를 호출
                g(3.0); // 컴파일 오류: g(double)가 삭제됨
                f(nullptr); // 컴파일 오류: f(std::nullptr_t)가 삭제됨
}
            ```
## 6.1.3 Default Parameters | Attributes [[attribute]]

- default parameter
    : 말그대로 디폴트 값(초기값)을 가지는 파라미터라고 할 수 있습니다
    - 다음과 같은 규칙을 따릅니다
        - 디폴트 파라미터는 다른 매개변수를 먼저 작성한다음 오른쪽에 작성합니다.
        - 한번 선언된 기본 매개변수 값은 변경될 수 없습니다.
        - 기본매개 변수 선언으로 컴파일시간 최적화, 중복코드 감소효과가 있습니다.
        ```
        void f(int a, int b = 20);
        void f(int a, int b){...}
        f(5);
        ```
- Function Attribute
    함수의 의도를 명확하게 표현하기 위한 목적으로 표준속성을 사용하여 함수를 표시할 수 있습니다.
    예를들어
        - [[nonereturn]] (C++11)함수가 반환값이 없음을 나타냅니다
        - [[deprecated]] (C++14) 함수 사용을 권장하지 않을 떄 나타내며, 사용시 컴파일 에러가 나타납니다
        - [[nodiscard]]  (C++17) ,[[nodiscard("reason")]] 함수의  반환값을 사용하지않으면(discard) 경고가 발생합니다
        
            - 참고로 Discard란 함수가 반환한 값을 무시하는 것을 말합니다

        - [[maybe_unused]] (C++17) 미사용 변수에대한 컴파일러 경고를 억제합니다.

        -참고: nodiscard는 clang c++17 이후 attribute extension이라 비쥬얼스튜디오에서는 컴파일에러없이 잘 동작했습니다.
       
      <img src="/images/nodiscardCompile.png" width="320" height="180">
        ```
        [[nodiscard]]
        double calculateInterest(double principal, double rate) {
        return principal * rate;
        }

        int main() {
            double principal = 10000.0;
            double rate = 0.05;

            calculateInterest(principal, rate);  

            double interest = calculateInterest(principal, rate);
            std::cout << "Calculated interest: " << interest << std::endl;

            return 0;
        }
        ```


        
## 6.1.4 **Function Pointers and Function Objects**
    - Function Pointers
       - 개요
            - C언어의 경우, Function Pointer를 활용해 제네릭프로그래밍(일반화 프로그래밍)을 가능하게 합니다 
            - 함수 포인터를 사용하여 다른 함수에 "함수를" 인수로 전달할 수 있으며, 이를통해 간접적인 함수 호출(Indirect call)이 가능합니다.
            - 다만, 함수 포인터를 사용하여 전달된 인수에 타입검사가 없고, 간접적인 호출이기 때문에, 함수 인라이닝이 불가능하여 성능에 영향을 줄 수없습니다.
        -코드예시
            ```
            #include <stdlib.h> // qsort
            int descending(const void* a, const void* b) {
                return *((const int*)a) > *((const int*)b);
            }

            // array: { 7, 5, 2, 1 
            int main() {

            int array[] = { 7, 2, 5, 1 };
            qsort(array, 4, sizeof(int), descending);
            }
            ```
    - Function Object (Functor)
        - 개요
            - 함수포인터의 몇가지 단점을 보완하기 위한 역할을 합니다., 
            - 함수처럼 동작하는 객체를 말합니다.
                - 객체가 마치 일반함수처럼 사용될 수 있다는 말 입니다.  
            - 이는 연산자 오버로딩을 통해 가능하며 operator()를 오버로딩 하여 구현합니다
        -코드예시
            ```
            class Adder {
            public:
                Adder(int y) : y(y) {}  // 생성자

                // 함수 호출 연산자 오버로딩
                int operator()(int x) {
                    return x + y;  // 입력 x에 y를 더해서 반환
                }

            private:
                int y;
            };

            int main() {
                Adder addFive(5);  // 5를 더하는 함수 객체 생성
                int result = addFive(3);  // 함수처럼 객체를 호출. 결과는 8
                std::cout << "Result: " << result << std::endl;  // "Result: 8" 출력
                return 0;
            }
            ```
        

## 6.2 **Lambda Expressions**

### 6.2.1 Lamda Expression
- 람다함수란(C+11) Inline local-scope function object
    - 로컬 스코프에서 유효한, 인라인 함수 오브젝트
    - 인라인 함수란, 일반적인 함수호출과정을 거치지 않고, 함수의 모든 코드를 호출된 곳에 바로 삽입하는 방식의 함수를 말합니다.
        - 호출시 걸리는 시간을 절약할 수 있다는 장점이 있으나, 호출과정에서 얻는 이점을 잃게되는 단점이 있습니다. 
        - 따라서 적은 코드량의 함수만을 인라인함수로 선언하는 것이 권장되는 편입니다.
- 람다함수 예시(교재꺼 아님)
```
int main() {
    std::vector<int> numbers = { 10, 20, 30, 40, 50 };
    // 인라인 람다 표현식: numbers 배열의 각 요소에 5를 더함
    std::for_each(numbers.begin(), numbers.end(), [](int& n) {
        n += 5;
        });
    for (int n : numbers) {
        std::cout << n << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

### 6.2.2 Capture List
- 캡쳐리스트란 람다 함수 바깥에 있는 변수들을 람다 함수 내부에서 사용할 수 있게 해주는 매커니즘을 말합니다.
    - 외부 스코프를 값으로 캡쳐 혹은 참조로 캡쳐(Value Capture, Reference Caputre)할 수 있습니다.
    - 이를 통해 람다 내부에서 외부 변수를 읽거나 수정할 수 있습니다. 
- 캡쳐리스트 옵션
    - [] 캡쳐 X
    - [=] 모든 외부변수를 값으로 캡쳐합니다.
    - [&] 모든 외부변수를 참조로 캡쳐합니다.
    - [var1]: var1만 값을 캡처합니다.
    - [&var2]: var2만 참조로 캡처합니다.
    - [var1, &var2]: var1은 값으로, var2는 참조로 캡처합니다.
    - [=, &var1]: var1을 제외한 모든 변수를 값으로 캡처하고, var1만 참조로 캡처합니다.
    - [&, var1]: var1을 제외한 모든 변수를 참조로 캡처하고, var1만 값을 캡처합니다.
    - constexpr로 선언된 변수는 컴파일 시간에 값이 결정되므로, 이러한 변수는 캡쳐선언 필요없이 람다함수에서 직접 사용가능합니다.

- 코드예시
    ```
    #pragma STDC FENV_ACCESS ON
    #include <algorithm>
    #include <iostream>


    int array[] = {11, 2, 5, 1 };

    int main() {

        int limit = 10;

        // 람다를 사용하여 배열에서 limit보다 큰 첫 번째 원소 찾기
        auto lambda1 = [=](int value) { return value > limit; };  // 모든 외부 변수를 값으로 캡처
        auto lambda2 = [&](int value) { return value > limit; };  // 모든 외부 변수를 참조로 캡처
        auto lambda3 = [limit](int value) { return value > limit; };  // "limit"만 값을 캡처
        auto lambda4 = [&limit](int value) { return value > limit; };  // "limit"만 참조로 캡처


        auto result = std::find_if(std::begin(array), std::end(array), lambda1);

        if (result != std::end(array)) {
            std::cout << "Found a value greater than " << limit << ": " << *result << std::endl;
            //11 출력
        }
        else {
            std::cout << "No value greater than " << limit << " found." << std::endl;
        }
    }
    ```
    -원본에 영향을 준 경우.
    ```
    int array[] = {15, 2, 5, 1 };

    int main() {


        int limitRef = 10;
        auto lambda2 = [&](int value) { limitRef += 1; return value > limitRef; };  
        auto result = std::find_if(std::begin(array), std::end(array), lambda2);

        if (result != std::end(array)) {
            std::cout << "Found a value greater than " << limitRef << ": " << *result << std::endl;
        //Found a value greater than 11: 15 이 출력된다. Ref캡쳐기때문에 원본에 영향을 줬다.
        }
    }
    ```


### 6.2.3 Parameters | Composability | constexpr/consteval
- Parameters
    - C++14 람다 표현식부터, 파라미터는 자동으로 추론될 수 있습니다.
    ```
    int value = 8;
	auto x = [](auto value) { return (value + 4); };
	std::cout << x(value); 12출력
    ```
    - C++14 안에서 초기화 변수 선언이 가능합니다.
    - 이는 다음과같을때 유용할 수 있습니다.
        - 초기화 선언 
            ```auto configure = [](double timeout =0.5f, bool loggingEnabled =false)
        - 조건문 만들기
        ```
        auto adjustValue = [](int base, int increment = 1) {
        return base + increment;
        };
        int value = 10;
        std::cout << "Adjusted Value: " << adjustValue(value, 5) << std::endl;
        std::cout << "Base Value: " << adjustValue(value) << std::endl; // Uses default increment
        ```
        - Fallback Value assignment
        ```
        auto processInput = [](const std::string& data, int maxLen = 100) {
        std::string trimmed = data.substr(0, maxLen);
        std::cout << "Processed data: " << trimmed << std::endl;
        ```
        - **즉 람다지만 일반적인 함수에서 디폴트 파라미터 선언으로 얻는 이점을 활용할 수 있게됩니다!** 

    };
    processInput("Very long data string that might need trimming");

        ```
    ```
	int value = 8;
	auto x = [](int i = 6) { return i + 4; };
	std::cout << x(value) << std::endl;
	std::cout << "i: "<< i << std::endl;
    ```

    - Composability
        - 람다안에 다르게 선언된 람다함수를 사용할 수 있습니다.
            ```
            auto lamda1 = [](int value) {return value + 4; };
            auto lamda2 = [](int value) {return value * 2; };

            auto lamda3 = [&](int value) {return lamda2(lamda1(value)); };;

            int value = 8;
            std::cout << "x: "<< lamda3(8); //"x: 24" 출력
            ```
        - 함수는 람다를 반환할 수 있으며, 다이나믹 디스패치(Dynamic Dispatch) 또한 가능합니다. 
                - 엄밀한 메소드 오버라이딩 관점에서의 다이나믹 디스패치라고 할 수 없으나,  런타임에서 어떤 메소드를 실행하지 결정할 수 있도록 만들 수 있다는 점에서 가능하다고 할 수 있습니다. 
            ```
            auto f(){
                return [](int value){return value+4;};
                auto lamda = f();
                std::cout<<lamda(2);
            }
            ```
        - constexpr | consteval  
            - constexpr(C++17) 
                - 람다표현식을 컴파일 시간에 평가 하도록 합니다. 즉 컴파일 이후에는 불변합니다.
                ```
                auto factorial = [] (int value) constexpr{
                    int ret = 1; 
                    for (int i =2; i <=; i++)
                    {
                        ret *= i;
                    }
                    return; 
                }

                ```
            - consteval (C++20)
                - 람다 표현식을 컴파일 타임에 평가되게 하며, 상수성(Constancy)를 강제하여, 조건 불충분시 컴파일에러를 발생시킵니다
               ```
            auto mul = [](int v) consteval { return v * 2; };

            int main() {
             const int val = 1;
            constexpr int result = mul(val);
            std::cout << "Result: " << result << std::endl; // Should output "Result: 20"
            }
               ```
    

### 6.2.4 template | mutable

    - lamda Expression, template
        - (C++20)에서 람다 표현식은 템플릿 매개변수를 사용할 수 있게되었습니다.
        - 이를 통해 람다 함수 내에서 다양한 타입에 대해 동작할 수 있습니다.
        ```
        int main() {
        // 산술타입인지를 평가합니다. 참고로 is_artimetic_v는 모든 정수, 부동소수점 타입을 포함합니다.
        auto lamda = []<typename T>(T value) requires std::is_arithmetic_v<T> {
            return value * 2;
        };

        auto v = lamda(3.4);
        struct A {} a; 
        //auto v = lamda(a)// 컴파일에러!!
}
        ```

    - mutable
        - 기본적으로 람다에서 캡쳐한 값은 수정할 수 없습니다.
        - 하지만 mutable 키워드를 사용하면 이러한 제약을 해제할 수 있습니다.
        ```
        int var = 1;
        auto lambda1 = [&]() { var = 4; }; // ok
        lambda1();
        std::cout << var; // print '4'
        // 아래처럼 기본적으로 var라는 값을 캡쳐했을때, 람다에서 직접적으로 수정하는건 기본적으로 불가능합니다!
        //auto lambda2 = [=](){ var = 3; }; // compile error
        // lambda operator() is const
        auto lambda3 = [=]() mutable { var = 3; }; // ok
        ```

### 6.2.5 [[nodiscard]] Attribute | Capture List and Classes 
    - [[nondiscard]]
        - **C++23** 부터, [[nondiscard]]속성을 추가할 수 있으며, 이는 해당 함수나 여기서는 **람다의 반환값**을 무시하면 안되는 중요한 값임을 나타내는 속성입니다.
        - nondiscard 속성을 활용하여, 잠재적인 오류를 예방하고, 개발자가 반환값을 의도적으로 확인하도록 유도 할 수 있습니다.
        ```
        int main() {

        auto lambda = [] [[nodiscard]] (){ return 4; };
        lambda(); // 이 코드는 컴파일러 경고를 발생시킵니다
        auto x = lambda(); // ok
        }
        ```
    - 람다 표현식에서 캡쳐리스트를 사용하여, 특정변수를 람다함수 내부에서 사용할 수 있도록합니다. 다음은 지역변수를 캡쳐하는 방법입니다
        1.[this] 'this' 포인트를 캡쳐하여, 현재 객체를 참조로 캡쳐합니다 
            - C++17 이전에는 명시적으로 사용해야 했습니다. 
        2.[x=x]
        3.[&x = x]
        4.[=]
        - 코드예시
            ```
        class A {
        public:
            int data = 1;
            

            auto f() {
                int var = 2; // 지역 변수4
                
            
                auto lambda1 = [=]() { return var; }; // var를 값으로 캡처, 2 반환

                auto lambda2 = [=]() { int var = 3; return var; }; // 최근 범위의 var를 사용, 3 반환

                auto lambda3 = [this]() { return data; }; // this를 통해 data 참조, 1 반환
                auto lambda4 = [*this]() { return data; }; // C++17부터 가능, *this를 통해 객체 복사, data 값 1 반환
                // auto lambda5 = [data]() { return data; };클래스 맴버인 data를 이런 문법으로 접근할 수 없음
                auto lambda6 = [data = data]() { return data; }; // data를 값으로 캡처하여 data 반환, 1 반환

                return lambda4;
            }
        };

        int main() {


            A a;
            auto lambda = a.f();  // a.f() 호출 결과를 lambda 변수에 저장
            std::cout << lambda() << std::endl;  // lambda를 실행하고 결과 출력
        }
	
        ```

## 7 Prerocessing
### 7.1 Preprocessor
- 개요 
    - 전처리기 지시문(Preprocessor Directive) 이란, 소스코드를 컴파일 하기 이전에, 어떻게 해석해야하는 것인지 컴파일러에게 알려주는 작업을 말합니다. 
    - Macro는 전처리기 지시문이며, 나머지 코드에서 식별자의 발생을 대체합니다. 
    코드예시
    ```
    // 매크로 정의
    #define PI 3.14159
    #define SQUARE(x) ((x) * (x))

    int main() {
        // 매크로 사용
        double radius = 5.0;
        double area = PI * SQUARE(radius);

        std::cout << "원의 반지름: " << radius << std::endl;
        std::cout << "원의 넓이: " << area << std::endl;

        return 0;
    }
    ```
    - 매크로는 다음과 같은 단점이 있기때문에 조심해서 사용해야합니다
        1. 직접 디버깅 할 수 없습니다.
        2. 예상못한 부작용을 가질 수 있습니다.
        3. 이름공간이나 범위가 없습니다. 
    - 전처리기 사용관련
        - Hash(#)로 시작하는 문을 포함합니다 
        - 예시
             ```
            #include "myfile.h" : 현재 파일에 코드를 삽입
            #define MACRO <expression> : 새로운 매크로정의
            #undef MACRO : 매크로 정의해제  // 안전을 위해 최대한 즉각적으로 정의를 해제해야합니다.
            ** 다중 줄 처리 시, 코드한줄 끝에 \ 기호를 삽입합니다. **
            #  define을 통해 들여쓰기가 가능합니다. 
    - 조건부 컴파일 문법은 다음과같습니다.
        - #if <condition> : 조건이 참일때 코드 실행
        - #elif <condition> : 이전 조건이 거짓이고 현재 조건이 참일 때 코드 실행
        - #else : 이전 조건들이 거짓일 때 코드 실행
        - #endif: 조건부 컴파일 블록 종료
        - #if defined(MACRO): 이전 조건이 거짓이고 현재 매크로가 정의되어있는지 확인
        - #elif defined(MACRO) : 이전 조건이 거짓이고 현재 매크로가 정의 되어있는지 확인
        - #if !defined(MACRO) : 매크로가 정의되어있지 않은지 확인
        - #elif !defined(MACRO) 이전 조건이 거짓이고 현재 매크로가 정의되어 있지 않은지 확인(C++23)



### Common Errors
- 매크로 사용시 발생하는 흔한 에러들입니다.
    1. 헤더파일 및 인클루드 이전에 매크로를 정의하면 안됩니다.
    ```
        #include <iostream>
        #define value  //매우위험
        #include "big_lib.hpp"

        int main() {
            std::cout << f(4)>>
        }
    ```
    2. 매크로 정의시 괄호를 사용해야합니다. 
    ```
    #include <iostream>
    #define SUB1(a,b) a-b// 잘못된사용
    #define SUB2(a,b) (a-b) //잘못된사용

    #define SUB#(a,b) ((a) -(b)) // 올바른사용
    ```
    3. 매크로는 컴파일 에러를 찾기가 매우 어렵습니다.
    4. 매크로 내용이 항상 평가되는 것은 아니므로 주의해야합니다
    ```
    #if defined(DEBUG)
    #define CHECK(EXPR) // EXPR을 이용해서 무언가를 수행
    void check(bool b) { /* b를 이용해서 무언가를 수행 */ }
    #else
    #define CHECK(EXPR) // 아무것도 수행하지 않음
    void check(bool) {} // 아무것도 수행하지 않음
    #endif
    ```
    5. 다중 줄 매크로에 중괄호를 사용하면 예기치 않게 동작합니다
    ```
    #define NUCLEAR_EXPLOSION
    {
    std::cout << "핵폭탄 투척">>;
    nuclear
    }
    
    int main(){
        bool never_happen = false;
        if(never_happen)
        NUCLEAR_EXLPOSION
    }
    ```

    6. 매크로는 항상 스코프를 명시해야합니다.
    7. 매크로는 부작용을 발생시킬 수 있습니다
    ```
    # define MIN(a, b) ((a) < (b) ? (a) : (b))
    int main() {
    int array1[] = { 1, 5, 2 };
    int array2[] = { 6, 3, 4 };
    int i = 0;
    int j = 0;
    int v1 = MIN(array1[i++], array2[j++]); // v1 = 5!!
    int v2 = MIN(array1[i++], array2[j++]); // undefined behavior/segmentation fault A
    }
    //대체시 다음과 같아지기때문에, 두번 증감연산자 실행 및, 배열인덱스를 벗어나기때문에 세그멘테이션 오류가발생힙니다. 
    ((array1[i++]) < (array2[j++]) ? (array1[i++]) : (array2[j++]))
    ```



- Source Location Macros
소스위치 매크로란, 컴파일 중인 줄 번호 혹은 파일이름과 같은 정보를 제공하는 매크로입니다.
종류는 다음과같습니다
 - __LINE__ : 현재 소스 코드 파일에서의 줄 번호를 나타내는 정수 값
 -__FILE__ : 컴파일 중인 소스파일의 이름을 나타내는 문장졀 입니다. 
 -__FUNCTION__: 현재 매크로 범위 내의 함수의 전체 시그니처를 나타내는 문자열 입니다.
 -__PRETTY_FUNCTION__:현재 매크로 범위 내의 함수 전체 시그니처를 나타내는 문자열 입니다. (GCC Clang)
 -__func__ 현재 매크로 범위 내의 함수 이름을 나타내는 C++11 키워드입니다.

- 코드예시
 ```
 # define MIN(a, b) ((a) < (b) ? (a) : (b))
        int main() {
            std::cout << __FILE__ << ":" << __LINE__ << std::endl; // '\MyCppProject\MyCppProject\Source.cpp:11' 출력
            std::cout << __FUNCTION__ << std::endl; // 'main' 출력
            std::cout << __func__ << std::endl; // 'main' 출력
    }
 ```

 - C++20에서는 소스 위치 매크로를 대체하기 위한 소스위치 유틸리티가 제공됩니다. 이를 사용해 매크로 기반 접근 방식을 대체 할 수 있습니다
 ```
 #include <source_location>
 current(): 소스 위치 정보 
 line(): 소스 코드 줄 번호
 column(): 소스 코드 열번호
 file_name(): 현재파일 이름
 function_name(): 현재 함수 이름 

 ```
- Stringizing Operator # | #error and #warning | #pragma

    - 문자열화 매크로 연산자 (#)
        - 해당하는  인수(arguments)를 따옴표로 둘러쌉니다.즉 인수를 문자열 상수로 변환하여 출력합니다.
        - 연산자를 활용하여 디버깅 메세지를 생성하는데도 사용 할 수 있습니다.
         ```
        #define INFO_MACRO(my_func) \
        { \
            my_func; \
            std::cout << "call " << #my_func << " at " \
                    << __FILE__ << ":" << __LINE__; \
        }

        void g(int) {}

        int main() {
            INFO_MACRO(g(3));
        }
         ```
    - #error and #warning 
        - #error "text" 
        컴파일러가 이를 파싱할 때 사용자가 지정한 오류 메세지를 컴파일 시간에 발생시키고, 컴파일 프로세스를 중지합니다
        - C++23 #warning "text" 
        사용자 지정 경고 메세지를 컴파일시간에 발생시키지만, 컴파일 프로세스를 중지시키지는 않습니다.
    - #pragma
        - #pragma는 컴파일러의 구현별 동작을 제어합니다. 일반적으로 이식성은 없습니다.
        - #pragma message "text" 이 지시문은 컴파일 시간에 정보 메세지를 표시합니다. 
        - #pragma GCC diagnostic warning "-WFormat" 이 지시문은 GCC 경고를 비활성화 합니다.
        - Pragma(<command>)(C++11) #define에 내장될 수 있는 키워등비니다.
        ```
        #define MY_MESSAGE \
        _Pragma("message(\"hello\")")
        ```

- Token-Pasting Operator ## | Variadic Macro

### Token-Pasting Operator (##)

토큰 결합매크로 연산자(##)는 두 개의 토큰을 결합합니다. (공백 없이)
사용법: tokenA##tokenB와 같이 사용하며, tokenA와 tokenB를 결합하여 새로운 식별자를 생성합니다.
예시
```
#define FUNC_GEN_A(tokenA, tokenB) \
void tokenA##tokenB() {}

#define FUNC_GEN_B(tokenA, tokenB) \
void tokenA##_##tokenB() {}

FUNC_GEN_A(my, function)
FUNC_GEN_B(my, function)
/*
예를 들어, FUNC_GEN_A(my, function)은 my와 function을 결합하여 새로운 함수 이름인 myfunction을 생성합니다.
*/

// 함수 호출
myfunction(); // FUNC_GEN_A에서 생성됨
my_function(); // FUNC_GEN_B에서 생성됨
Variadic Macro
```
개요: 가변 인자 매크로는 C++11에서 도입된 특수한 매크로로, 변수 개수의 인수를 허용합니다. (쉼표로 구분됨)
동작: 매크로 치환 목록에서 VA ARGS의 각 발생은 전달된 인수로 대체됩니다.
```
void f(int a) { printf("%d", a); }
void f(int a, int b) { printf("%d %d", a, b); }
void f(int a, int b, int c) { printf("%d %d %d", a, b, c); }

#define PRINT(...) \
f(__VA_ARGS__);

PRINT(1, 2)
PRINT(1, 2, 3)
```
### Macro Trick

목적: 숫자 리터럴을 문자열 리터럴로 변환합니다.
동기: 정수를 문자열로 변환하는 것을 피하려는 목적으로 성능을 개선합니다.
```
#define TO_LITERAL_AUX(x) #x
#define TO_LITERAL(x) TO_LITERAL_AUX(x)

int main() {
    int x1 = 3 * 10;
    int y1 = __LINE__ + 4;
    char x2[] = TO_LITERAL(3);
    char
```
이것들은 매크로를 사용하여 코드를 유연하게 만들고, 특히 가변 인자 매크로를 통해 함수의 다양한 매개변수에 대응할 수 있습니다. 토큰 붙이기 연산자(##)는 여러 개의 매크로를 조합하여 새로운 기능을 만드는 데 사용됩니다.