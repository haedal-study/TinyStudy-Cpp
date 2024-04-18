### 6.1 Pass by-Value | Pass by-Pointer |Pass by-Reference

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
- 6.1.1 Pass-by-Value
             
    - 함수 호출 시, 인자는 함수 파라미터로 복제됩니다.
    - 함수 내부에서 파라미터는, 함수 외부의 변수와 분리됩니다.
    - 즉 파라미터에서의 값 수정은 원래 값에 영향을 주지 않습니다.
    ```
    void f(int x){
        x =10; // x라는 변수가 들어와도 x값은 바뀌지않습니다.
    }
    ```

- 6.1.2 Pass by Pointer(Call By Pointer)
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

- 6.1.3 참조에 의한 전달(Call by Reference)
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

    

### 6.1 Pass by-Value | Pass by-Pointer |Pass by-Reference

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
- 6.1.1 Pass-by-Value
             
    - 함수 호출 시, 인자는 함수 파라미터로 복제됩니다.
    - 함수 내부에서 파라미터는, 함수 외부의 변수와 분리됩니다.
    - 즉 파라미터에서의 값 수정은 원래 값에 영향을 주지 않습니다.
    ```
        void f(int x)
        {
            x =10; // x라는 변수가 들어와도 x값은 바뀌지않습니다.
        }
    ```

- 6.1.2 Pass by Pointer(Call By Pointer)
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

- 6.1.3 참조에 의한 전달(Call by Reference)
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

    

## 6.2 Function Signature and Overloading | Overloading and =delete
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
## 6.3 Default Parameters | Attributes [[attribute]]

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