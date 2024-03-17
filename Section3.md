# Section 3

## 3.2 Integral Data Types
    ### 3.1 Fixed width Integers, 고정 너비 정수 타입
        - 고정너비 타입들은 <cstdint> 헤더에 정의 되어 있습니다. 
        - 이 타입들은 아키텍쳐에 관계 없이 일정한 크기를 가집니다. 
            - 이는 예전에 제조사, 운영체제, 프로세서에 따라 정수비트의 크기가 다른 문제를 해결하기 위한 대안이라고 볼 수 있습니다. 
        - int8_t, uint8_t ... int64_t, uint64_t 의 고정너비 타입이 있으며, C++에서 널리 사용되는 int와 unsigned와는 다릅니다.
        - 참고로 int*_t 타입은 실제 독립적은 타입이 아니고, 기본타입에 대한 별칭입니다.
        - C++ 표준은 char , short, int, long, long등 기본적인 정수 타입을 제공합니다. 
        - ** 그러나 C++표준이 int*_t 타입을 기본타입과 일대일로 매핑하는 것은 아닙니다.
            - 즉 시스템에서 정확한 크기의 기본타입을 제공하지 않는다면 동작하지 않을 수 있다는 뜻이며, 이식성을 생각할때 고려요소 중 한가지가 될 수 있겠습니다.
        - 주의: I/O Stream에서는 uint8_t와 int8_t를 char로 해석합니다.
            - 근데 제 컴퓨터에서는 cout<<var구문에서 부터 에러가 나는 것 같습니다..
            ```
                int8_t var;
                cin >> var; // read '2'
                cout << var; // print '2'
                int a = var * 2;
                cout << a; // 100 이 출력됩니다 !!
            ```
    ### 3.2 size_t, ptrdiff_t
        - <stddef.h> 헤더에 정의된 size_t와 ptrdiff_t 는 아키텍쳐에 따라 가장 큰 크기를 저장할 수 있는 타입의 별칭 입니다.
            - 여기서 가장 큰 크기라 함은 예를들어 64bit 아키텍쳐에서는 8btyte를, 32bit 아키텍쳐에서는 4byte를 사용할 수 있게 한다는 것입니다.
        - size_t는 부호 없는 정수 타입이며, 최소 16비트 크기를 가지게 됩니다.
        - ptrdiff_t 는 size_t 의 signed 버전이며, 포인터차이를 계산할 때 사용되게 됩니다
            - 메모리 내의 두 위치사이의 거리 (오프셋) 을 나타낼 때 사용합   니다. 부호있는 타입으로 음수값을 가질 수 있어, 한 포인터가 다른 포인터 보다 앞에 있느 경우 음수 값을 반환할 수 도 있습니다. 
        - size_t의 리턴타입은, size_t 이며, 크기를 나타내는데 흔히 사용됩니다.
        - C++ 23에서는 size_t와 ptrdiff_t를 위해 uz?UZ와 z/Z를 사용합니다. 

## 3.2 Signed/Unsigned Integer Characteristics
    - Signed와 Unsigned 정수는 동일한 하드웨어를 사용하지면, Semantic에서 큰 차이를 보입니다.
    - 기본개념은 다음과 같습니다.
        오버플로우 : 산술연산 길이가 단어 길이를 초과하여, 표현할 수 있는 가장 큰 양수/음수 값보다 큰 경우를 말합니다.
        Wraparound : 
            - 특정 범위가 넘어가면 다시 처음으로 (해당타입이 표현할 수 있는 가장 작은 수) 돌아가는 것을 의미합니다.
            - 산술 연산의 결과가 2^N  모듈로로 감소하며, N은 여기서 단어의 비트 수 입니다. 
            - 즉 오버플로우가 발생하면 랩어라운드가 일어납니다. 최대값을 초과하는 산술 연산의 결과는 0 부터 다시 시작하며, 이것은 모듈로 연산으로 다시 reduced 되었다고 표현합니다. 
            ``` 
            uint8_t num = 255; // uint8_t는 0부터 255까지 값을 가질 수 있습니다.
            num = num + 1;     // 여기서 랩어라운드가 발생합니다 256을 가질수 없으니 다시 0으로 돌아갑니다.2^N(256) 범위 안에서 연산이 //이루어지기 때문입니다. 
            std::cout << static_cast<int>(num) << std::endl; // 출력 결과는 0입니다
            return 0;
            ``` 
## 3.3 Promotion and Truncation
    - 큰 타입으로 프로모션 되는 경우에는 부호를 유지합니다.
    - Truncation 이란, 변수가 더 작은 데이터 타입으로 변환될 때 일어나는 과정입니다. 
        Truncation의 경우, 더 작은 타입으로의 Truncation은 모듈로 연산을 사용합니다. 즉 Wraparound가 일어나게됩니다. 
        ```
        int x = 65537; // 2^16 + 1
        int16_t y = x; // 결과는 2^16 모듈로 연산
        cout << y;     // print 1

        int z = 32769; // 2^15 + 1 (int16_t 범위를 넘음)
        int16_t w = z; // 결과는 2^16 모듈로 연산, 즉 32768에서 + 1 값을  Wraparound 한 -32767 이 됩니다. 
        cout << w;     // print -32767
        ```

## 3.4 Undefined Behavior 
    : 프로그래머는 변수의 범위를 주의깊게 관리해야합니다. 
    정의되지않은 행동이 일어날 수 있는 경우는 다음과 같습니다.
        - 정수가 자신이 표현할 수 있는 정수 범위를 넘어서는 경우
        - 부호 있는 정수 타입에서, 비트연산 시 
            - 예를 들어, 쉬프트연산으로 sign bit를 변경하는 것은 특히 위험 할 수 있습니다. 
        - 데이터 타입이 표현할 수 있는 비트수 보다 큰 시프트 연산을 수행하는 경우
        - 암묵적 형 변환에서, 부호없는 타입을 넘어서는 값으로 변환하려는 경우 (정수 오버플로우)
    - 정말 안좋은 예시
        - 아래 예시는 컴파일러 최적화 활성화시, 오버플로우를 유발합니다. 
        - i*100000000 연산시 오버플로우 발생 == Undefined Behavior -> 코드 경로 제거 가능 -> 즉 컴파일러가 루프 탈출 조건 체크를 제거할 가능성 -> 무한루프 발생
        ```
                int main() {
        for (int i = 0; i < 4; ++i)
        std::cout << i * 1000000000 << std::endl;
        }
        // with optimizations, it is an infinite loop
        // --> 1000000000 * i > INT_MAX
        // undefined behavior!!
        // the compiler translates the multiplication constant into an addition
        ```
        - ![alt text](image-1.png)
            -직접 돌려보니 무한루프가 나오진 않았다, 컴파일러 최적화가 적용이 안된 걸 수 있음.

    안전한 루프 작성방법
        - 변수가 그 타입의 최대값에 도달하지 않도록 주의합니다.
        - 루프 조건과 증감 연산자에서 오버플로우가 발생하지 않도록 충분한 여유를 줘야합니다.
        - 변수가 최대값에 근접할 수 있는 연산을 할때는 오버플로우를 감지하고 처리할 수 있는 로직을 추가해야합니다.
            -예시로<limits> 헤더를 추가하여, 연산 수행시 오버플로우 가능성을 검사할 수있습니다. 
            - std::numeric_limits<int>::max() 를 사용할 수 있습니다. 


## 3.5 IEEE FLoating-Point Standard Overview
    - IEEE754
        - Institute of Electrical and Electronics Enginners
        - 부동 소수점의 연산을 위해 채택된 국제적인 표준을 의미합니다.
        - 이진 형식, 연산 규칙, 반올림, 예외처리를 표준화 합니다. 
        - 다시말해 위와 같은 항목을 표준화 하여, 컴퓨터간에 숫자를 일관적으로 처리하도록 정의한 것 입니다.
        - 표준연혁
            - 첫 릴리즈 : 1985
            - 두번째 릴리즈 : 2008 Add 16-bit, 128bit, 256 bit등의 다양한 비트길이의 부동 소수점 타입 추가
            - 세번째 릴리즈: 2019, 최소/최대 처리방식에 대해 규칙을 명시합니다 
        - C/C++ 은 IEEE754 표준을 따릅니다 (<numveric_limits>)
    - 비표준 방식 (Non-Standardized in C++/IEEE)
        - TensorFloat-32
            -딥러닝 응용프로그램을 위한 특수한 포맷의 부동소수점 입니다.
            - NVIDIA의 최신 GPU 아키텍쳐에서 사용되는 부동소수점 형식입니다. 
            - https://blogs.nvidia.com/blog/tensorfloat-32-precision-format/
        -Posit
            - John Gustafson에 의해 2017년 제안됨, unum III라고도 불립니다.
            - 지수부와 가수부의 가변 너비를 가지는 부동소수점 값을 표현합니다.
            - 실험적 플랫폼에서 구현되었습니다.
        _Microscaling Formats(MX)
            - AMD, Arm, Intel, Meta, Microsoft, NVIDIA, Qualcomm이 정의한 저정밀도 부동소수점 형식입니다.
            - FP8, FP6, FP4, (MX)INT8을 포함합니다.
        Fixed Point
            - 임베디드 시스템에서 많이 사용됩니다.
            - 소숫점 이하 자리가 고정된 형식입니다.
            - 인접한 숫자 간격이 항상 동일합니다.
                - 인접한 숫자간격이 동일하지 않을 수도 있다는 것은, 100.1과 100.2 사이의 차이가 0.1,0.2사이의 간격과 정밀도 이슈로 인해 동일하지 않을 수 있다는 것을의미합니다.
                - 큰 숫자일수록 간격이 더 커질 수 있는데,
            - 값의 범위가 부동 소수점 수에 비해서 상당히 제한적입니다. 
            ![alt text](image-2.png)