# C++ Data model
![C++ Data model](Images/C++DataModel.png)
# 고정 길이 정수형
- C++에서는 고정 길이 정수형을 제공하며, 어느 아키텍처에서든 같은 사이즈를 가짐

![C++ Data model](Images/FixedWidthIntegers.png)
- 네이티브 타입 대신 고정 길이 정수 선호
- `int*_t`는 실제 타입이 아님
    - 4가지 오버로드가 있음 (`int8_t`, `int16_t`, `int32_t`, `int64_t`)
- I/O Stream에서 `uint8_t`와 `int8_t`는 `char`처럼 정수형으로 인식하지 않음
<br></br>
# size_t와 ptrdiff_t
- 현재 아키텍처에서 저장할 수 있는 가장 큰 값을 나타내는 별칭 데이터 타입
- size_t
    - 부호가 없는 정수형 타입
    - `sizeof()`의 반환타입이며, 일반적으로 크기를 표현할 때 사용
    - C++23부터 `uz`/`UZ` 리터럴
- ptrdiff_t
    - `size_t`의 부호 있는 정수형 타입
    - 포인터 뺄셈을 계산하는 데 사용
    - C++23부터 `z`/`Z` 리터럴
- 32-bit 아키텍처에서는 4bytes, 64-bit 아키텍처에서는 8bytes
<br></br>
## 자료
- https://github.com/federico-busato/Modern-CPP-Programming/blob/master/03.Basic_Concepts_II.pdf
- https://en.cppreference.com/w/cpp/header/cstddef
