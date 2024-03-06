# 개요
본 스터디는 [Federico Busato의 Modern Cpp Programming](https://github.com/federico-busato/Modern-CPP-Programming)을 기반으로 진행되는 스터디입니다.
적은 분량이라도 **매일 꾸준히 하는 것**을 목표로 진행됩니다. 꾸준히 한다는 것은 휴일이 없다는 얘기입니다.
C++을 배우고 싶은 사람 누구나 참여가 가능하며, 검증된 자료로 진행하기에 체계적으로 C++을 배워나갈 수 있습니다.

# 참여 인원
| 참여자 | 참여 동기 | 각오 |
| ----- | --- | --- |
| [최선문](https://github.com/choiseonmun) | - | - |
| [이동규](https://github.com/Dong-kyu-Lee) | C++을 확실하게 공부하고 싶습니다. | 꾸준히 열심히 스터디에 참여하겠습니다. |
| [안예준](https://github.com/Zel-da) | 언리얼 엔진을 공부하고 있는데 C++ 지식이 부족해서 이를 보완하고자 참여했습니다. | C++을 제대로 다룰 수 있을만큼 많이 배우고 노력하겠습니다. |
| [신중현](https://github.com/JunghyeonShin) | - | - |
| [남궁영빈](https://github.com/YB970902) | 언리얼 엔진을 공부하고 싶었는데 C++을 혼자하려니 의지력이 부족해서 이를 극복하고자 참여했습니다. | 열심히 해서 끝까지 안 빠지고 진도 잘 나가도록 하겠습니다! |
| [한승희](https://github.com/kaylahee) | 교환학생 이후 C++을 잊은 상태라 이를 복습하고자 스터디를 참여하게 되었습니다. | 꾸준히 열심히 해서 밀리지 않겠습니다. |
| [공민석](https://github.com/Mati-as) | 취업 후 언리얼 엔진을 배우고 싶어 C++을 공부하고 싶었는데, 독학하려니 막막해서 참여했습니다. | 만족스러운 결과물을 내도록 하겠습니다. |
| [주진수](https://github.com/weweweme) | 앞으로의 커리어 발전을 위해서 C++을 아는 것이 중요하다고 생각하고 이를 인덱싱하고 싶습니다. | 진도 밀리지 않고 꾸준히 뒤따라가겠습니다. | 
| [최희지](https://github.com/yoreleihee) | C#말고 다른 언어를 다루고 싶어 참여를 결정했습니다. | C++도 C#만큼 다뤄보겠습니다! |
| [백승수](https://github.com/BaekSeungSu) | 언리얼 엔진을 공부 중이라 C++을 잘하고 싶어 참여했습니다. | 낙오하는 일 없이 끝까지 가도록 하겠습니다. |
| [최서연](https://github.com/csy-59) | 이번에 복학하는 김에 다양한 걸 공부하고 싶었고, 언리얼 엔진 엔지니어가 되고자 C++을 배우려 참여했습니다. | 꾸준히 열심히 따라가도록 하겠습니다. |


# 커리큘럼
* 크게 6개의 섹션으로 나뉘며, 하나의 섹션이 끝나면 일정 기간 동안 리프레시 기간을 부여합니다.
* 하루에 한 항목씩 해나가면 됩니다.
* 커리큘럼은 스터디 진행 도중 바뀔 수 있습니다.

## 1차 - C++ 기초(1~6장)
### 1장 : Introduction
- Area of Application and Popularity
- C++ Philosophy
- C++ Weaknessses

### 2장 : Basic Concepts I - Fundamental Types 
- Hello World
- Fundamental Types Overview
- Conversion Rules | auto Declaration
- C++ Operators

### 3장 : Basic Concepts II - Integral and Floating-point Types
**Integral Data Types**
- Fixed Width Integers | size_t and ptrdiff_t
- Signed/Unsigned Integer Characteristics
- Promotion, Truncation
- Undefined Behaviour

**Floating-point Types and Arithmetic**
- IEEE Floating-point Standard and Other Representations
- Normal/Denormal | Infinity | NaN; Not a Number
- Machine Epsilon | ULP; Units at the Last Place
- Cheatsheet
- Arithmetic Properties | Detect Floating-point Errors

**Floating-point Issues**
- Catastrophic Cancellation
- Floating-point Comparison

### 4장 : Basic Concepts III - Entities and Control Flow
- Entities | Declaration and Definition
- Enumerators
- struct, Bitfield and union

**Control Flow**
- if statement | for and while Loops | Range-based for Loop
- switch | goto | Avoid Unused Variable Warning [[maybe_unused]]

### 5장 : Basic Concepts IV - Memory Management
**Heap and Stack**
- Stack Memory
- new, delete
- Non-allocating Placement Allocation | Non-throwing Allocation
- Memory Leak

**Initialization**
- Variable Initialization | Uniform Initialization | Fixed-size Array Initialization
- Structure Initialization | Dynamic Memory Initialization

**Pointers and References**
- Pointer Operations
- Address-of operator &
- Reference

**Constants, Literals, const, constexpr, consteval, constinit**
- Constants and Literals | const
- constexpr
- consteval | constinit | if constexpr
- std::is_constant_evaluated() | if consteval

- volatile Keyword

**Explicit Type Conversion**
- static_cast, const_cast, reintepret_cast
- Type Punning

- sizeof Operator

### 6장 : Basic Concepts V - Functions and Preprocessing
**Functions**
- Pass by-Value | Pass by-Pointer |Pass by-Reference
- Function Signature and Overloading | Overloading and =delete
- Default Parameters | Attributes [[attribute]]

- **Function Pointers and Function Objects**

**Lambda Expressions**
- Capture List
- Parameters | Composability | constexpr/consteval
- template | mutable
- [[nodiscard]] | Capture List and Classes

**Preprocessing**
- Preprocessors
- Common Errors
- Source Location Macros
- Condition Compiling Macros
- Stringizing Operator # | #error and #warning | #pragma
- Token-Pasting Operator ## | Variadic Macro

## 2차 - 객체지향 프로그래밍, 일반화 프로그래밍(7~10장)

## 3차 - 빌드(11장 ~ 12장)

## 4차 - 에코시스템(13장 ~ 15장)

## 5차 - STL(16장 ~ 19장)

## 6차 - 최적화(20장 ~ 22장)

# 진행 방식
- `Github 계정 이름`으로 브랜치를 만듭니다.
- 본인 브랜치에서 `Github 계정 이름`으로 된 디렉토리를 만들고, 매일 주어진 분량을 공부하여 본 레포의 원격 브랜치로 푸시합니다.
