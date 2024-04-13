# Day-38

## Type Punning

### Pointer Aliasing

- 한 포인터가 같은 메모리 위치를 가리키면 다른 포인터에 별칭을 붙임

### Type Punning

- 타입 퍼닝은 프로그래밍 언어의 유형 시스템을 우회하여 공식 언어의 범위 내에서 어려운 또는 불가능한 효과를 달성하는 것을 의미
- 메모리의 바이트를 다른 방식으로 해석하여 데이터를 조작하는 기술을 가리킴
- C++에서 reinterpret_cast나 union등을 사용하여 변수의 유형을 적절하게 변환하지 않고도 메모리의 내용을 다른 유형으로 해석할 수 있음
    
    ```cpp
    int main() {
        int intValue = 42;
        float floatValue = *reinterpret_cast<float*>(&intValue);
        return 0;
    }
    // 정수형 변수의 주소를 부동 소수점 포인터로 캐스팅
    // 해당 메모리 위치의 데이터를 부동 소수점으로 해석
    ```
    
    ```cpp
    union TypePun {
        int intValue;
        float floatValue;
    };
    
    int main() {
        TypePun pun;
        pun.intValue = 42;
        float floatValue = pun.floatValue;
        return 0;
    }
    
    // intValue와 floatValue는 같은 메모리 위치를 공유
    // intValue에 값을 할당한 후 floatValue로 읽으면
    // 같은 비트 패턴을 가진 부동 소수점 값으로 해석
    ```
    
- gcc compiler옵션
    - fstrict-aliasing 옵션은 type-punning을 허용하지 않겠다는 의미
    - fno-strict-aliasing 옵션은 허용하겠다는 의미

### memcpy and std::bit cast

- 정의되지 않은 동작을 피하는 올바른 방법은 memcpy를 사용하는 것
    
    ```cpp
    float v1 = 32.3f;
    unsigned v2;
    std::memcpy(&v2, &v1, sizeof(float));
    // v1, v2
    ```
    
- C++20에서는 std::bit cast 함수가 제공됨
- 이 함수는 reinterpret cast와 유사한 기능을 제공하지만, 더 안전하고 명시적인 방식으로 동작함
    
    ```cpp
    #include<bit>
    
    float v1 = 32.3f;
    unsigned v2 = std::bit_cast<unsigned>(v1);
    
    // std::bit_cast 함수를 사용해서 float v1을 unsigned int v2로
    // 안전하게 변환
    // 메모리 비트 패턴을 유지하므로 reinterpret_cast보다 안전함
    ```
