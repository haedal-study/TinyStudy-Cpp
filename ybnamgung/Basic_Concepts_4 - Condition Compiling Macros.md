# Basic_Concepts_4 - Condition Compiling Macros

## Condition Compiling Macros

C/C++ 버전에 따라 코드 선택

- #if defined(__cplusplus) : C++ code
- #if __cplusplus = 199711L : ISO C++ 1998/2003
- #if __cplusplus == 201103L : ISO C++ 2011
- #if __cplusplus == 201402L : ISO C++ 2014
- #if __cplusplus == 201703L : ISO C++ 2017

컴파일러에 따라 코드 선택

- #if defined(__GNUG__) : gcc/g++
- #if defined(__clang__) : clang/clang++
- #ifdefine(_MSC*_*VER) : Microsoft Visual C++

운영체제 또는 환경에 따라 코드 선택

- #if defined(_WIN64) : Window 64-bit
- #if defined(__linux__) : Linux
- #if defined(__APPLE__) : Mac OS
- #if defined(__MINGW32__) : MinGW 32-bit