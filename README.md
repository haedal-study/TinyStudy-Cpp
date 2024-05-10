# 개요
본 스터디는 [Federico Busato의 Modern Cpp Programming](https://github.com/federico-busato/Modern-CPP-Programming)을 기반으로 진행되는 스터디입니다.
적은 분량이라도 **매일 꾸준히 하는 것**을 목표로 진행됩니다. 꾸준히 한다는 것은 휴일이 없다는 얘기입니다.
C++을 배우고 싶은 사람 누구나 참여가 가능하며, 검증된 자료로 진행하기에 체계적으로 C++을 배워나갈 수 있습니다.

# 참여 인원
| 참여자 | 참여 동기 | 각오 |
| ----- | --- | --- |
| [최선문](https://github.com/choiseonmun) | - | - |
| [이동규](https://github.com/Dong-kyu-Lee) | C++을 확실하게 공부하고 싶습니다. | 꾸준히 열심히 스터디에 참여하겠습니다. |
| [남궁영빈](https://github.com/YB970902) | 언리얼 엔진을 공부하고 싶었는데 C++을 혼자하려니 의지력이 부족해서 이를 극복하고자 참여했습니다. | 열심히 해서 끝까지 안 빠지고 진도 잘 나가도록 하겠습니다! |
| [공민석](https://github.com/Mati-as) | 취업 후 언리얼 엔진을 배우고 싶어 C++을 공부하고 싶었는데, 독학하려니 막막해서 참여했습니다. | 만족스러운 결과물을 내도록 하겠습니다. |
| [주진수](https://github.com/weweweme) | 앞으로의 커리어 발전을 위해서 C++을 아는 것이 중요하다고 생각하고 이를 인덱싱하고 싶습니다. | 진도 밀리지 않고 꾸준히 뒤따라가겠습니다. | 
| [최희지](https://github.com/yoreleihee) | C#말고 다른 언어를 다루고 싶어 참여를 결정했습니다. | C++도 C#만큼 다뤄보겠습니다! |

# 커리큘럼
* 크게 6개의 섹션으로 나뉘며, 하나의 섹션이 끝나면 일정 기간 동안 리프레시 기간을 부여합니다.
* 하루에 한 항목씩 해나가면 됩니다.
* 커리큘럼은 스터디 진행 도중 바뀔 수 있습니다.

## 1차 - C++ 기초(1~6장)
### 1장 : Introduction
- 3.07 : Area of Application and Popularity
- 3.08 : C++ Philosophy
- 3.09 : C++ Weaknessses

### 2장 : Basic Concepts I - Fundamental Types 
- 3.10 : Hello World
- 3.11 : Fundamental Types Overview
- 3.12 : Conversion Rules | auto Declaration
- 3.13 : C++ Operators

### 3장 : Basic Concepts II - Integral and Floating-point Types
**Integral Data Types**
- 3.14 : Fixed Width Integers | size_t and ptrdiff_t
- 3.15 : Signed/Unsigned Integer Characteristics | Promotion, Truncation
- 3.16 : Undefined Behaviour

**Floating-point Types and Arithmetic**
- 3.17 : IEEE Floating-point Standard and Other Representations
- 3.18 : Normal/Denormal | Infinity | NaN; Not a Number
- 3.19 : Machine Epsilon | ULP; Units at the Last Place
- 3.20 : Cheatsheet
- 3.21 : Arithmetic Properties | Detect Floating-point Errors

**Floating-point Issues**
- 3.22 : Catastrophic Cancellation
- 3.23 : Floating-point Comparison

### 4장 : Basic Concepts III - Entities and Control Flow
- 3.24 : Entities | Declaration and Definition
- 3.25 : Enumerators
- 3.26 : struct, Bitfield and union

**Control Flow**
- 3.27 : if statement | for and while Loops | Range-based for Loop
- 3.28 : switch | goto | Avoid Unused Variable Warning [[maybe_unused]]

### 5장 : Basic Concepts IV - Memory Management
**Heap and Stack**
- 3.29 : Stack Memory
- 3.30 : new, delete
- 3.31 : Non-allocating Placement Allocation | Non-throwing Allocation
- 4.01 : Memory Leak

**Initialization**
- 4.02 : Variable Initialization | Uniform Initialization | Fixed-size Array Initialization
- 4.03 : Structure Initialization | Dynamic Memory Initialization

**Pointers and References**
- 4.04 : Pointer Operations
- 4.05 : Address-of operator &
- 4.06 : Reference

**Constants, Literals, const, constexpr, consteval, constinit**
- 4.07 : Constants and Literals | const
- 4.08 : constexpr
- 4.09 : consteval | constinit | if constexpr
- 4.10 : std::is_constant_evaluated() | if consteval

- 4.11 : volatile Keyword

**Explicit Type Conversion**
- 4.12 : static_cast, const_cast, reintepret_cast
- 4.13 : Type Punning

- 4.14 : sizeof Operator

### 6장 : Basic Concepts V - Functions and Preprocessing
**Functions**
- 4.15 : Pass by-Value | Pass by-Pointer |Pass by-Reference
- 4.16 : Function Signature and Overloading | Overloading and =delete
- 4.17 : Default Parameters | Attributes [[attribute]]

- 4.18 : **Function Pointers and Function Objects**

**Lambda Expressions**
- 4.19 : Capture List
- 4.20 : Parameters | Composability | constexpr/consteval
- 4.21 : template | mutable
- 4.22 : [[nodiscard]] | Capture List and Classes

**Preprocessing**
- 4.23 : Preprocessors
- 4.24 : Common Errors
- 4.25 : Source Location Macros
- 4.26 : Condition Compiling Macros
- 4.27 : Stringizing Operator # | #error and #warning | #pragma
- 4.28 : Token-Pasting Operator ## | Variadic Macro

## 2차 - 객체지향 프로그래밍, 일반화 프로그래밍(7~10장)
### 7장 : Object-Oriendted Programming I - Class Concepts
- 5.16(~10/66) : RAII Idiom
- 5.17(~19/66) : Class Hierarchy | Access Specifiers

**Class Constructor**
- 5.18(~28/66) : Default Constructor | Class Initialization
- 5.19(~37/66) : Uniform Initilization for Objects | Delegate Consturctor | explicit Keyword | [[nodiscard]] and Classes

- 5.20(~47/66) : Copy Constructor | Class Destructor

**class Keywords**
- 5.21(~56/66) : default | this | static
- 5.22(~66/66) : const | mutable | using | friend | delete
  
### 8장 : Object-Oriented Programming II - Polymorphism and Operator Overloading
**Polymorphism**
- 5.23(~7/66) : Overview
- 5.24(~14/64) : virtual Methods | Virtual Table
- 5.25(~23/64) : override | final | Common Erros | Pure Virtual Method | Abstract Class and Interface

- 5.26(~32/64) : Inheritance Casting and Run-time Type Identification

**Operator Overloading**
- 5.27(~39/64) : Comparison Operator | Spaceship Operator
- 5.28(~45/64) : Subscript Operator | Multidimensional Subscript Operator | Function Call Operator | static operator() and static operator[] | Conversion Operator
- 5.29(~53/64) : Return Type Overloading Resolution | Increment and Decrement Operator | Assignment Operator | Stream Operator | Operator Notes

**C++ Object Layout**
- 5.30(~59/64) : Aggregate | Trivial Class
- 5.31(~64/64) : Standard-Layout Class | POD | Hierarchy

### 9장 : Templates and Meta-programming I - Function Templates and Compile-Time Utilities
**Function Template**
- 6.01(~8/48) : Overview
- 6.02(~14/48) : Template Instantiation | Template Parameters (~14/48)
- 6.03(~20/48) : Template Parameters - Default Value | Overloading | Specialization

- 6.04(~27/48) : Template Variable | Template Parameter Types
- 6.05(~33/48) : Compile-Time Utilities

**Type Traits**
- 6.06(~37/48) : Overview
- 6.07(~42/48) : Type Traits Library
- 6.08(~48/48) : Type Manipulation
  
### 10장 : Templates and Meta-programming II - Class Templates and SFINAE
- 6.09(~10/80) : Class Template
- 6.10(~19/80) : CTAD; Constructor Template Automatic Deduction
- 6.11(~29/80) : Class Template - Advanced Concepts
- 6.12(~36/80) : Template Meta-programming

**SFINAE: Substitution Failure Is Not An Error**
- 6.13(~39/80) : Overview
- 6.14(~46/80) : Function SFINAE
- 6.15(~52/80) : Class SFINAE

**Variadic Templates**
- 6.16(~61/80) : Overview
- 6/17(~69/80) : Folding Expression | Variadic Class Template

**C++20 Concepts**
- 6.18(~74/80) : Overview | concept Keyword | reuquires Clause
- 6.19(~80/80) : requires Expression | requires Expression + Clause | requires Clause + Expression | requires and constexpr | Nested requires

## 3차 - 빌드(11장 ~ 12장)

## 4차 - 에코시스템(13장 ~ 15장)

## 5차 - STL(16장 ~ 19장)

## 6차 - 최적화(20장 ~ 22장)

# 진행 방식
- `Github 계정 이름`으로 브랜치를 만듭니다.
- 본인 브랜치에서 `Github 계정 이름`으로 된 디렉토리를 만들고, 매일 주어진 분량을 공부하여 본 레포의 원격 브랜치로 푸시합니다.
