# Day-53

## Token-Pasting Operator ##, Variadic Macro

### Token-Pasting Operator ##

- 토큰 연결(또는 붙여넣기) 매크로 연산자 (##)를 사용하면 공백을 남기지 않고 두 개의 토큰을 결합할 수 있음
    
    ```cpp
    #include <iostream>
    #define FUNC_GEN_A(tokenA, tokenB)\
    		void tokenA##tokenB(){}
    #define FUNC_GEN_B(tokenA, tokenB)\
    		void tokenA##_##tokenB(){}
    using namespace std;
    
    FUNC_GEN_A(my, function)
    FUNC_GEN_B(my, function)
    
    int main() {
    	myfunction();
    	my_function();
    
    	return 0;
    }
    ```
    

### Variadic Macro

- C++11 가변 매크로는 쉼표로 구분된 가변 인수를 허용하는 특수 매크로임
- 매크로 대체 목록에서 특수 식별자 “__VA_ARGS__”의 각 발생은 전달된 인수로 대체됨
    
    ```cpp
    #include <iostream>
    using namespace std;
    
    void f(int a) { cout << a << endl; }
    void f(int a, int b) { cout << a << ", " << b << endl; }
    void f(int a, int b, int c) { cout << a << ", " << b << ", " << c << endl; }
    
    #define PRINT(...)\
    		f(__VA_ARGS__);
    
    int main() {
    	PRINT(1);
    	PRINT(1, 2);
    	PRINT(1, 2, 3);
    
    	return 0;
    }
    ```
