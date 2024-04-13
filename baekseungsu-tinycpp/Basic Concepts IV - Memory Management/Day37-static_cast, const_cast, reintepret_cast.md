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
