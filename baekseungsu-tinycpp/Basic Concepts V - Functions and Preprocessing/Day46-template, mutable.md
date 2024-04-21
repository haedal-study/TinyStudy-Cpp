# Day-46

## Lambda Expressions
## template, mutable

### template Lambda Expression

- C++20 람다 표현식은 template를 지원하며 requires 절을 지원함
- requires
    - 타입이 가져야 하는 요구 조건을 정의하는 문법
    - requires 키워드 다음에는 다음과 같은 형식이 올 수 있음
        1. 하나의 명명된 concept
        2. 명명된 concept의 논리곱(&&)이나 논리합(||)
        3. requires 표현식 같은 컴파일 시점 술어
    
    ```cpp
    #include <iostream>
    #include <functional>
    using namespace std;
    
    auto lambda = []<typename T>(T value)requires is_arithmetic_v<T> {
        return value * 2;
    };
    
    auto lambda2 = []<typename T>(T v1, T v2) {
        return v1 * v2;
    };
    
    int main() 
    {
        auto v = lambda(3.4); // v 
    
        auto x = lambda2(1, 2);
    
        // auto y = lambda2(1, 2.f); // 형식과 다르기 때문에 컴파일 에러
    
        cout << v << endl;
    
        cout << x << endl;
    
        return 0;
    }
    
    // 출력
    // 6.8
    // 2
    ```
    

### mutable Lambda Expression

- 람다 캡처는 by-const-value
- mutable 지정자를 사용하면 람다가 값으로 캡처된 매개변수를 수정할 수 있음
    
    ```cpp
    #include <iostream>
    #include <functional>
    using namespace std;
    
    int main() 
    {
        int var = 1;
    
        auto lambda1 = [&]() {var = 4; }; // 참조 캡처
        lambda1();
        cout << var << endl; // print '4'
    
       // auto lambda2 = [=]() {var = 3; }; // 컴파일 에러
       
        auto lambda3 = [=]() mutable{var = 3; };
        lambda3();
        cout << var << endl; // print '4'
    
        return 0;
    }
    ```
    
- 람다식을 거치며 외부 변수의 값도 변경하고 싶은 경우 → &
- 람다식 내에서 value만 받고 밖 변수는 그대로 두고 싶은 경우 → mutable
