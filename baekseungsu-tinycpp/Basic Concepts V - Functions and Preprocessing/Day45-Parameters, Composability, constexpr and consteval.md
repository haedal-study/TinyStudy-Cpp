# Day-45

## Lambda Expressions
## Parameters, Composability, constexpr/consteval

### **Parameters**

- C++14 람다 표현식 매개변수는 자동으로 추론될 수 있음
    
    ```cpp
    #include <iostream>
    #include<algorithm>
    using namespace std;
    
    int main()
    {
    	auto x = [](auto value) {return value + 4; };
    	
    	cout << x(5) << endl;
    
    	return 0;
    }
    
    // 출력
    // 9
    ```
    
- C++14 람다 표현식 매개변수는 초기화 가능
    
    ```cpp
    #include <iostream>
    #include<algorithm>
    using namespace std;
    
    int main()
    {
    	auto x = [](int i = 6) {return i + 4; };
    	
    	cout << x() << endl;
    
    	return 0;
    }
    // 출력
    // 10
    ```
    

### Composability

- 람다 표현식은 다음과 같이 구성 가능
    
    ```cpp
    #include <iostream>
    #include<algorithm>
    using namespace std;
    
    int main()
    {
    	auto lambda1 = [](int value) { return value + 4; };
    	auto lambda2 = [](int value) { return value * 2; };
    	auto lambda3 = [&](int value) { return lambda2(lambda1(value)); };
    	// return (value + 4) * 2;
    	
    	cout << lambda3(2) << endl;
    
    	return 0;
    }
    ```
    
- 함수는 람다를 반환할 수 있음
    
    ```cpp
    #include <iostream>
    #include<algorithm>
    using namespace std;
    
    auto f() {
    	return [](int value) {return value + 4; };
    }
    
    int main()
    {
    	auto lambda = f();
    	cout << lambda(2);
    
    	return 0;
    }
    // 출력
    // 6
    ```
    
- dynamic dispatch도 가능하다
    - dynamic dispatch
        - 런타임에 호출할 다형성 연산(함수)의 구현을 선택하는 프로세스

### constexpr/consteval Lambda Expression

- C++17 람다 표현식은 constexpr을 지원함
- C++20 람다 표현식은 consteval을 지원함
    
    ```cpp
    #include <iostream>
    #include <functional>
    using namespace std;
    auto factorial = [](int value) constexpr {
        int ret = 1;
        for (int i = 2; i <= value; i++)
            ret *= i;
        return ret;
        };
    
    int main() {
    
        auto mul = [](int v) consteval { return v * 2; };
        constexpr int v1 = factorial(4) + mul(5); 
    
        cout << v1 << endl;
    
        return 0;
    }
    
    // 출력
    // 34 : '24 + '10'
    ```
