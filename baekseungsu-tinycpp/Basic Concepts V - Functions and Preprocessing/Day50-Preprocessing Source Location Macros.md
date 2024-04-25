# Day-50

## Preprocessing
## Source Location Macros

### Source Location Macros

- __LINE__
    - 컴파일 중인 소스 코드 파일의 현재 줄을 나타내는 정수 값
- __FILE__
    - 컴파일 중인 소스 파일의 이름이 포함된 문자열 리터럴
- __FUNCTION__
    - 비표준(gcc, clang)
    - ‘매크로 범위’의 함수 이름을 포함하는 문자열 리터럴
- __PRETTY_FUNCTION__
    - 비표준(gcc, clang)
    - ‘매크로 범위’에 있느 함수의 전체 시그니처를 포함하는 문자열 리터럴
- __func__
    - C++11 키워드
    - ‘매크로 범위’에 있는 함수 이름이 포함된 문자열
- example.cpp
    
    ```cpp
    #include<iostream>
    using namespace std;
    
    void f(int p) {
    	cout << __FILE__ << ":" << __LINE__ << endl;;
    	cout << __FUNCTION__ << endl;;
    	cout << __func__ << endl;;
    }
    
    template<typename T>
    float g(T p) {
    	cout << __PRETTY_FUNCTION__;
    	return 0.0f;
    }
    
    void g1() { g(3); }
    
    int main() {
    	f(3);
    	cout << "-----------------------------------" << endl;
    	g1();
    	return 0;
    }
    ```
    
- C++20은 매크로 기반 접근 방식을 대체할 수 있는 소스 위치 유틸리티를 제공함
    - #include <source_location>
    - cuurent()
        - 소스 위치 정보 가져오기
        - static member
    - line()
        - 소스 코드 라인
    - column()
        - 라인 열
    - file_name()
        - 현재 파일 이름
    - function_name()
        - 현재 함수 이름
    
    ```cpp
    #include<iostream>
    #include<source_location>
    using namespace std;
    
    void f(source_location s = source_location::current()) {
    	cout << "function : " << s.function_name() << ", line" << s.line() << endl;
    }
    
    int main() {
    	f();
    	return 0;
    }
    
    // 출력
    // function : int __cdecl main(void), line10
    ```
