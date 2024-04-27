# Day-51

## Preprocessing
## Condition Compiling Macros

### C/C++ 버전에 따른 코드 선택

- __cplusplus
    - 현재 사용중인 C++ 표준 년도를 나타냄
- #if defined(__cplusplus)
    - C/C++ 버전을 가져옴
    - C와 C++간의 링커 재사용을 위해 사용
    - 기본적으로 __cplusplus의 값을 199711L을 반환
- #if cplusplus == 199711L
    - ISO C++ 1998/2003
- #if cplusplus == 201103L
    - ISO C++ 2011*
- #if cplusplus == 201402L
    - ISO C++ 2014*
- #if cplusplus == 201703L
    - ISO C++ 2017

### 컴파일러에 따른 선택

- #if defined(__GNUG__)
    - C++ 컴파일러(gcc/g++) 코드 선택
- #if defined(__clang__)
    - clang 컴파일러 코드 선택
- #if defined(_MSC_VER)
    - Microsoft Visual C++ 컴파일러 코드 선택

### 운영 체제에 따른 선택

- 특정 운영 체제에서만 실행될 코드를 결정할 수 있음
- #if defined(_WIN64)
    - OS가 Windows 64비트인 경우
- #if defined(__linux__)
    - OS가 리눅스인 경우
- #if defined(__APPLE__)
    - OS가 Mac인 경우
- #if defined(__MINGW32__)
    - OS가 MinGW 32비트인 경우
- 기타 등등..
