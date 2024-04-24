# Day-49

## Preprocessing
## Common Errors

### Conmmon Error 1

- 헤더파일을 포함한 이후에 매크로를 정의해야 함
    
    ```cpp
    #include <iostream>
    #define value
    #include "big_lib.h"
    // #define value를 해당위치에 삽입해야 함
    using namespace std;
    
    int main() 
    {
        cout << f(4); // 7이 출력되지않고 3이 출력
    
        return 0;
    }
    ```
    
    ```cpp
    // big_lib.h
    #pragma once
    
    int f(int value) {
    	return value + 3;
    }
    ```
    
- 매크로가 헤더이 있는 경우 이 문제를 확인하기 매우 어려움

### Common Error 2

- #if defined 는 매크로 가시성과 관련된 버그를 유발할 수 있음
    
    ```cpp
    #include <iostream>
    #include "macro_definition.h"
    // ENABLE_DEBUG를 정의하는 헤더를 추가하는 것을 잊지 말아야 함
    using namespace std;
    
    #if defined(ENABLE_DEBUG)
    int f(int v) { cout << v << endl; return v * 3; }
    #else
    int f(int v) { return v * 3; }
    #endif
    
    int main() 
    {
        cout << f(3) << endl;
        return 0;
    }
    ```
    

### Common Error 3

- 매크로 정의에서 괄호를 사용하는 것을 잊지 말 것
    
    ```cpp
    # include <iostream>
    # define SUB1(a, b) a - b // WRONG
    # define SUB2(a, b) (a - b) // WRONG
    # define SUB3(a, b) ((a) - (b)) // correct
    
    int main() {
    	std::cout << (5 * SUB1(2, 1)); // print 9 not 5!!
    	std::cout << SUB2(3 + 3, 2 + 2); // print 6 not 2!!
    	std::cout << SUB3(3 + 3, 2 + 2); // print 2
    }
    ```
    

### Common Error 4

- 매크로는 컴파일 오류를 찾기 어렵게 만듬
    
    ```cpp
    # include <iostream>
    #define F(a) std::cout << a << std::endl;\
    			 std::cout << a+3 << std::endl;\
    			 return v;
    		
    
    int main() {
    	F(3)  // 이 줄에서 컴파일 에러
    }
    ```
    

### Common Error 5

- 매크로는 표현식 평가와 관련된 버그를 발생시킬 수 있음
    
    ```cpp
    #include<iostream>
    
    # if defined(DEBUG)
    # define CHECK(EXPR) // do something with EXPR
    void check(bool b) { std::cout << b << std::endl; }
    # else
    # define CHECK(EXPR) // do nothing
    void check(bool) {} // do nothing
    # endif
    bool f() { return true; }
    
    int main() {
    	check(f());
    	CHECK(f()) // <-- problem here
    }
    ```
    
- DEBUG가 정의되지 않으면
    - f()는 매크로를 사용하여 평가되지 않음

### Common Error 6

- 여러 줄을 묶어서 매크로를 정의할 경우 중괄호 대신 “\”를 사용할 것
    
    ```cpp
    #include <iostream>
    
    #define PRINT_NUM3(x) std::cout << x << std::endl; \
                          std::cout << x + 2 << std::endl; \
                          std::cout << x + 4 << std::endl;;
    
    int main()
    {
        PRINT_NUM3(10);
     
        return 0;
    }
    
    // 출력
    // 10
    // 12
    // 14
    ```
    

### Common Error 7

- 매크로에는 범위가 없음
    
    ```cpp
    # include <iostream>
    void f() {
    # define value 4
    	std::cout << value << std::endl;
    }
    int main() {
    	f(); // 4
    	std::cout << value << std::endl; // 4
    # define value 3
    	f(); // 4
    	std::cout << value << std::endl; // 3
    }
    ```
    
- 일반적으로 컴파일러는 동일한 매크로에 대한 여러 정의에 대해 경고를 표시

### Common Error 8

- 매크로는 부작용을 일으킬 수 있음
    
    ```cpp
    #include<iostream>
    # define MIN(a, b) ((a) < (b) ? (a) : (b))
    using namespace std;
    
    int main() {
    	int array1[] = { 1, 5, 2 };
    	int array2[] = { 6, 3, 4 };
    	int i = 0;
    	int j = 0;
    	int v1 = MIN(array1[i++], array2[j++]); // v1 = 5!!
    	int v2 = MIN(array1[i++], array2[j++]); // undefined behavior, segmentation fault
    	
    	cout << v1 << endl;
    	cout << v2 << endl;
    }
    
    // 출력
    // 5
    // unpredictable results 값
    ```
    

### Common Error 9

- 매크로 자체가 undefined behavior을 가질 수 있음
    
    ```cpp
    #include<iostream>
    
    # define MY_MACRO defined(EXTERNAL_MACRO)
    
    # if MY_MACRO
    # define MY_VALUE 1
    # else
    # define MY_VALUE 0
    # endif
    using namespace std;
    
    int f() { return MY_VALUE; } // undefined behavior
    
    int main() {
    	cout << f() << endl;
    }
    ```
