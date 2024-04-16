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

    

