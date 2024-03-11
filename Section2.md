## 2.1 Hello World

- C 언어에서의 printf의 경우는 다음과 같습니다. 

```

int main() {

printf("Hello World!\n");

}

```

- cout 은 C++의 표준 출력 스트림입니다.
  - cout은 iostream의 일부이며 , Character output의 약자입니다.
  - 여기서 스트림이란, 프로그램에서 출력자치로 데이터가 '흐르는' 방식을 일컫습니다. 즉 데이터가 전송되거나 수신되는 방법을 추상화 한 것 입니다.

```

#include <iostream>

int main() {

        std:cout << "Hello World!\n";

    }

```

- 아래는 global std를 이용해 cout을 사용한 경우 입니다.

```

#include <iostream>

using namespace std;

int main() {

    cout << "Hello World!\n";

}

```

- 아래는 입출력 스트림에 대한 예제 코드 입니다.

```

#include <iostream>

int main() {

    int a = 4;

    double b = 3.0;

    char c[] = "hello";

    std::cout << a << "" << b << "" << C << "\n"

}

```

  
  

## 2.2 Fundamental Types Overview

1 Arithmetic Types

1.1 Integral 

|            Native Type | bytes | Range                  | Fixed width types <cstdint> |
| ---------------------: | :---: | :--------------------- | --------------------------- |
|                   bool |   1   | true,false             |                             |
|                   char |   1   | implementation defined |                             |
|            signed char |   1   | -128 ~ 127             | int8_t                      |
|          unsigned char |   1   | 0 ~ 255                | uint8_t                     |
|                  short |   2   | -2^15 ~ 2^15 - 1       | int16_t                     |
|         unsigned short |   2   | 0 ~ 2^16-1             | uint16_t                    |
|                    int |   4   | _2^31 ~2^31 -1         | int32_t                     |
|           unsigned int |   4   | 0 ~ 2^32 -1            | uint32_t                    |
|               long int |  4/8  |                        | int32_t/int64_t             |
|      long unsigned int |  4/8  |                        | uint_t/uint64_t             |
|          long long int |   8   | -2^63 ~ 2^63-1         | int64_t                     |
| long long unsigned int |   8   | 0 ~ 2^64 -1            | uint_t                      |

- 고정 너비 변수 (Fixed width Types <cstint>)
	- 정수자료형은 왜 고정된 크기를 갖고 있지 않은가?
		참고:[[02.03. C++ | 고정너비 정수(Fixed-width integers)란? (tistory.com)](https://heroine-day.tistory.com/23)]
	- <cstdint> 헤더는 C++11에서 새로 도입되었으며, 정확한 비트수를 갖는 정수타입을 제공합니다.
	- 예를들어 표에서 int16_t는 1비트 크기를 가진 부호있는 정수를 의미합니다.
 
![[Pasted image 20240311195041.png]]

1.2 Floating-Point
![[Pasted image 20240311200054.png]]


1.3 Short name
|            Signed Type | short name |
| ---------------------: | :---: |
|            signed char |   /   |
|       signed short int |   short   |
|             signed int |   int  |
|        signed long int |  long  |
|   signed long long int |  long long|


1.3 suffix
![[Pasted image 20240311200106.png]]

```
#include <iostream>
#include <cstdint> 

int main() {
   
    // Integer literals
    int myInt = 2; 
    unsigned int myUnsignedInt = 3u; 
    long myLong = 8l; 
    unsigned long myUnsignedLong = 2ul; 
    long long myLongLong = 4ll; 
    unsigned long long myUnsignedLongLong = 7ull; 

    // Floating-point literals
    float myFloat = 3.0f; 
    double myDouble = 3.0; 
    long double myLongDouble = 3.0l; 

    // Fixed width integer types from <cstdint>
    std::int32_t myInt32 = 2;
    std::int64_t myInt64 = 2ll;

    std::cout << "myInt: " << myInt << "\n";
    std::cout << "myUnsignedInt: " << myUnsignedInt << "\n";
    std::cout << "myLong: " << myLong << "\n";
    std::cout << "myUnsignedLong: " << myUnsignedLong << "\n";
    std::cout << "myLongLong: " << myLongLong << "\n";
    std::cout << "myUnsignedLongLong: " << myUnsignedLongLong << "\n";
    std::cout << "myFloat: " << myFloat << "\n";
    std::cout << "myDouble: " << myDouble << "\n";
    std::cout << "myLongDouble: " << myLongDouble << "\n";
    std::cout << "myInt32: " << myInt32 << "\n";
    std::cout << "myInt64: " << myInt64 << "\n";

    return 0;

}

```
1.4 prefix
![[Pasted image 20240311202544.png]]


1.5 기타연산자
	-long double
	- reduced precision floating-point supports before C++ 23:
		- 정밀도는 낮지만, 대역폭 사용을 줄이기 위해 메모리사용량이 적은 부동소수점 연산 수행 데이터 타입입니다. 
		- 특정 현대 CPU나 GPU에서는 더 적은 메모리를 사용하는 half 타입도 지원합니다. OpenGL,Photoshop,Lightroom 같은 그래픽 툴에서도 지원합니다.
	-C++은 128비트 인티저를 지원하지 않습니다 (특정 아키텍쳐가 지원해도 C++은 지원하지 않습니다.)


1.6 void type
	- void는 불완전하며(정의되지 않은), 값이 없는 타입입니다.
	- 함수에서는 반환형이 없는경우를 지칭합니다.
	- C에서는 sizeof(void) 는 1로 컴파일 되나, C++에서는 sizeof(void)는 컴파일 되지 않습니다.
	 ```
	 int main() {
	 sizeof(void);
	 }
     ```


1.7 nullptr Keyword
- C++11 부터 널 포인터를 지칭하기위해 NULL 대신 nullptr을 사용합니다. 
```
int* p1 = NULL;
int* p2 = nullptr;

int n1 = NULL;
int n2 = nullptr // 정수형으로 변환불가능 하여 컴파일에러가 발생합니다. 
```



1.8 summary
- Fundamental types 혹은 빌트인 원시 연산자는 세개의 큰 카테고리로 구분됩니다
	- Integers
	- Floating-points
	- void, nullptr
- alias
	- typedef, using
- struct/class, array, union