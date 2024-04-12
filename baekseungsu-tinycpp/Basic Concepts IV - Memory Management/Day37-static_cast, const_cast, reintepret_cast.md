# Day-37

## Explicit Type Conversion
## static_cast, const_cast, reintepret_cast

### Cast

- 옛날 방식 cast : (type)value
- 새로운 방식 cast
    - static_cast
        - static_cast<바꾸려는 타입>(대상)
        - static_cast<new_type>(expression)
        - 컴파일 타임에 타입 검사를 수행
        - 타입간 우발적이거나 안전하지 않은 변환을 방지
        - 가장 안전한 형 변환
        
        ```cpp
        #include<iostream>
        
        using namespace std;
        int main(){
        	double d = 11.2;
        	float f = 5.1f;
        	
        	int tmp_int;
        	float tmp_float;
        	
        	tmp_int = static_cast<int>(d);
        	cout << tmp_int;  // print 11
        	
        	tmp_float = static_cast<float>(d);
        	cout << tmp_float; // print 11.2
        }
        	
        ```
        
    - const_cast
        - const_cast<바꾸려는 타입>(대상)
        - const_cast<new_type>(expression)
        - 포인터 또는 참조형의 const를 잠깐 제거해 주는데 사용
        - volatile 키워드를 잠깐동안 제거해 주는데도 사용 가능
        
        ```cpp
        #include<iostream>
        
        using namespace std;
        int main() {
        	char str[] = "pointer";
        	const char* ptr = str;
        	cout << "berfore : " << str << "\n";
        
        	char* c = const_cast<char*>(ptr);
        	c[0] = 'q';
        	cout << "after : " << str << endl;
        
        	// befor : pointer
        	// after : qointer
        	return 0;
        }
        ```
        
    - reinterpret cast
        - reinterpret_cast<바꾸려는 타입>(대상)
        - reinterpret_cast<new_type>(expression)
        - 임의의 포인터 타입끼리 변환을 허용
        - 정수형을 포인터로 바꿀 수 있음
            - 정수 값이 포인터의 절대 주소로 들어감 → 위험
        
        ```cpp
        #include<iostream>
        #include<cstdio>
        using namespace std;
        
        struct Cube {
        	int a;
        };
        
        int main() {
        	int a = 71234561;
        	
        	//1. int -> int * 로 타입캐스팅
        	//변수 a의 값을 절대주소로 받는 포인터 ptr1
        	//이 경우에는 주소 111번지를 가리키고 있는 poiner가 됨
        	//111번지가 어느곳을 가리킬지 모르기 때문에 위험
        	int* ptr1;
        	ptr1 = reinterpret_cast<int*>(a);
        	
        	cout << ptr1 << endl;
        	
        	return 0;
        }
        
        ```
        
- const_cast와 reinterpret_cast는 컴파일러에 의해 일련의 명령어로 변환되지만, 이 변환은 직접적으로 CPU 명령어 하나로 매핑되지 않음

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
  
