# 개요
본 스터디는 [Federico Busato의 Modern Cpp Programming](https://github.com/federico-busato/Modern-CPP-Programming)을 기반으로 진행되는 스터디입니다.
적은 분량이라도 **매일 꾸준히 하는 것**을 목표로 진행됩니다. 꾸준히 한다는 것은 휴일이 없다는 얘기입니다.
C++을 배우고 싶은 사람 누구나 참여가 가능하며, 검증된 자료로 진행하기에 체계적으로 C++을 배워나갈 수 있습니다.

# 참여 인원
<table>
  <tbody>
    <tr>
      <td align="center" valign="top" width="14.28%"><img src="https://avatars.githubusercontent.com/u/17216686?v=4" width="100px;" alt="choiseonmun"/><br /><sub><a href="https://github.com/choiseonmun"><b>choiseonmun</b></a></sub><br /></td>
      <td align="center" valign="top" width="14.28%"><img src="https://avatars.githubusercontent.com/u/65526713?v=4" width="100px;" alt="Dong-kyu-Lee"/><br /><sub><a href="https://github.com/Dong-kyu-Lee"><b>Dong-kyu-Lee</b></a></sub><br /></td>
      <td align="center" valign="top" width="14.28%"><img src="https://avatars.githubusercontent.com/u/66552780?v=4" width="100px;" alt="Zel-da"/><br /><sub><a href="https://github.com/Zel-da"><b>Zel-da</b></a></sub><br /></td>
      <td align="center" valign="top" width="14.28%"><img src="https://avatars.githubusercontent.com/u/77572658?v=4" width="100px;" alt="Choi Seoyeon"/><br /><sub><a href="https://github.com/csy-59"><b>Choi Seoyeon</b></a></sub><br /></td>
      <td align="center" valign="top" width="14.28%"><img src="https://avatars.githubusercontent.com/u/91855427?v=4" width="100px;" alt="JunghyeonShin"/><br /><sub><a href="https://github.com/JunghyeonShin"><b>JunghyeonShin</b></a></sub><br /></td>
    </tr>
    <tr>
      <td align="center" valign="top" width="14.28%"><img src="https://avatars.githubusercontent.com/u/92175769?v=4" width="100px;" alt="kaylahee"/><br /><sub><a href="https://github.com/kaylahee"><b>kaylahee</b></a></sub><br /></td>
      <td align="center" valign="top" width="14.28%"><img src="https://avatars.githubusercontent.com/u/120005151?v=4" width="100px;" alt="Mati Kong"/><br /><sub><a href="https://github.com/Mati-as"><b>Mati Kong</b></a></sub><br /></td>
      <td align="center" valign="top" width="14.28%"><img src="https://avatars.githubusercontent.com/u/120005202?v=4" width="100px;" alt="주진수"/><br /><sub><a href="https://github.com/weweweme"><b>주진수</b></a></sub><br /></td>
      <td align="center" valign="top" width="14.28%"><img src="https://avatars.githubusercontent.com/u/120005259?v=4" width="100px;" alt="yoreleihee"/><br /><sub><a href="https://github.com/yoreleihee"><b>yoreleihee</b></a></sub><br /></td>
    </tr>
  </tbody>
</table>

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
