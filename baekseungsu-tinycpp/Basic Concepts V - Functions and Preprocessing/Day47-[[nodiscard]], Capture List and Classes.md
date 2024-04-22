# Day-47

## Lambda Expressions
## [[nodiscard]], Capture List and Classes

### [[nodiscard]] Attribute

- C++23에서는 람다식에 [[nodiscard]] 속성을 추가할 수 있음
    
    ```cpp
    #include <iostream>
    #include <functional>
    using namespace std;
    
    int main() 
    {
        auto lambda = [] [[nodiscard]] (){ return 4; };
    
        lambda(); // warning, 컴파일 시점에 알려줌
    
        auto x = lambda();
    
        return 0;
    }
    ```
    

### Capture List and Classes

- [this] : 현재 객체 (*this) by-reference를 캡처함 (C++17)
- [x=x] : 현재 객체 멤버 x by-value를 캡처함 (C++14)
- [&x=x] : 현재 객체 멤버 x by-referece를 캡처함 (C++14)
- [=] : this 포인터를 값으로 캡처하는 디폴트 캡처가 폐지됨 (C++20)
    
    ```cpp
    #include <iostream>
    #include <functional>
    using namespace std;
    
    struct Test {
        int num1 = 5;
    
        auto AddNum1() {
            [ = ,*this]()mutable {num1 += 5; }(); // C++17부터 가능
        }
    
        auto AddNum2() {
            [= , this]() {num1 += 10; }(); // C++20부터 가능
        }
    
        auto AddNum3() {
            [&num1 = num1]() {num1 += 15; }();
        }
    
    };
    
    int main() 
    {
        Test t1;
        t1.AddNum1();
        cout << t1.num1 << endl; // 5, 복사 캡처, num1의 값 변경 X
    
        t1.AddNum2();
        cout << t1.num1 << endl; // 15, 참조 캡처, num1의 값 변경
    
        t1.AddNum3();
        cout << t1.num1 << endl; // 30, 참조 캡처, num1의 값 변경
    
        return 0;
    }
    ```
