# Day-52

## Preprocessing
## Stringizing Operator #, #error and #warning, #pragma

### Stringizing Operator #

- 문자열화 매크로 연산자(#)를 사용하면 해당 실제 인수가 큰 따옴표로 묶임
    
    ```cpp
    #include<iostream>
    #define STRING_MACRO(string) #string
    using namespace std;
    
    int main() {
    	cout << STRING_MACRO(hihi) << endl;
    }
    ```
    

### #error and #warning

- #error “text”
    - 컴파일할 때 컴파일러가 파싱하고 컴파일 프로세스를 중지할 때 사용자가 지정한 에러 메시지를 출력함
    
    ```cpp
    #ifdef __cpluslplus
    #include <iostream>
    #else
    #error c++에서만 사용 가능한 헤더 파일
    #endif
    
    using namespace std;
    
    int main() {
    
    }
    ```
    
- C++23 #warning “text”
    - 컴파일 프로세스를 중지하지 않고 컴파일 시 컴파일러가 구문을 분석할 때 사용자가 지정한 경고 메시지를 출력하는 지시어

### #pragma

- 컴파일러의 구현별 동작을 제어
- 일반적으로 이식 불가능
- #pragma message "text"
    - 컴파일 타임 정보 메시지 표시
    
    ```cpp
    #include <iostream>
    using namespace std;
    
    #pragma message("example")
    
    int main() {
    	return 0;
    }
    ```
    
- #pragma GCC diagnostic warning "-Wformat"
    - GCC 경고 비활성화 하기
    
    ```cpp
    #pragma GCC diagnostic error "-Wuninitialized"
        foo(a);
    
    #pragma GCC diagnostic push
    #pragma GCC diagnostic ignored "-Wuninitialized"
        foo(b);
    #pragma GCC diagnostic pop
    
        foo(c);
    #pragma GCC diagnostic pop 
    
        foo(d);
    ```
    
- _Pragma(<command>) C++11
    - 키워드
    - #define에 포함 가능
    
    ```cpp
    #include <iostream>
    #include <omp.h>
    // #define OMP_PARALLEL_FOR #pragma omp parallel for // 에러
    #define OMP_PARALLEL_FOR _Pragma("omp parallel for")
    // 위와같이 _Pragma를 사용 가능
    using namespace std;
    
    int main() {
    	OMP_PARALLEL_FOR
    	for (int i = 0; i < 10; i++) {
    		cout << "1";
    	}
    	return 0;
    }
    ```
