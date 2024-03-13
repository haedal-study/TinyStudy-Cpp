# Fixed Width Integers
-  C++은 고정 길이 정수 타입을 제공하며, 이 타입들은 어떤 아키텍트에서도 같은 크기를 갖는다. (cstdint)
```
int8_t, uint8_t
int16_t, uint16_t
int32_t, uint32_t
int64_t, uint64_t
```
- 이 타입들은 '실제적'인 타입들이 아니다.
- 그저 적절한 기본 타입들의 typedef 일 뿐이다.
- 기본 C++은 1 대 1 매핑을 보장하지 않는다.
	- 기본 타입은 5개(char, short, int, long, long long)
	- int*_t 타입들은 4개(int8_t, int16_t, int32_t, int64_t)
- I/O 스트림에서는 uint8_t와  int8_t를 정수 타입이 아닌 `char`로 변환한다.
```
int8_t var;
cin >> var; // '2'를 입력 받음
cout << var; // '2'를 출력, var은 아스키 코드로 '2', 즉 50

int a = var * 2; // var의 실제 값은 50, a는 100
cout << a; // 100 출력
```
---
# size_t and ptrdiff_t
- `size_t`와 `ptrdiff_t`는 현제 아키텍쳐에서 가장 크게 표현될 수 있는 타입에 대한 저장이 가능한 별칭 데이터 타입이다. (cstddef)
- `size_t`는 부호가 없는 타입이다. (최소 16비트)
- `ptrdiff_t`는 `size_t`의 부호가 있는 버전이며, 컴퓨팅 포인터의 차이를 위해 사용된다.
- `size_t`는 `sizeof()`의 반환 타입이며, 크기를 대표하기 위해 사용된다.
- `size_t`와 `ptrdiff_t`은 32비트 아키텍쳐에서는 4바이트며, 64비트 아키텍쳐에서는 8바이트다
- C++23에서는 `size_t`에 대한 리터럴 값 `uz`와 `UZ`, `ptrdiff_t`에 대한 리터럴 값 `z`와 `Z`를 추가했다.