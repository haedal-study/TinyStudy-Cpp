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