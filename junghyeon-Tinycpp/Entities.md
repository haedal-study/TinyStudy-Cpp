# Entity
- C++ 프로그램 구성
    - 언어별 키워드
    - 식별자
    - 연산자의 나열로 정의된 식
    - 리터럴
- 엔터티 (Entity)
    - 값, 객체, 참조, 함수, 열거, 타입, 클래스, 템플릿, 또는 네임스페이스
    - 전처리기 메크로는 C++ 엔터티가 아님
<br></br>
# 선언과 정의
- 선언 (Declaration)
    - 타입과 속성을 나타내는 식별자를 가진 엔터티를 소개
    - 컴파일러와 링커가 해당 식별자에 대한 참조를 수락하는 데 필요
    - 엔터티는 여러 번 선언할 수 있음
- 정의 (Definition)
    - 선언의 구현체
    - 엔터티의 속성과 동작을 정의
## 예제
- 함수
```
void f(int a, char* b); // function declaration
void f(int a, char*) {  // function definition
    ...                 // "b" can be omitted if not used
}

void f(int a, char* b); // function declaration
                        // multiple declarations is valid
f(3, "abc");            // usage

void g();               // function declaration
g();                    // linking error "g" is not defined
```
- 구조체
    - 구체적인 완성이 없는 선언은 **불완전한 타입**(incomplett type)
```
struct A;   // declaration 1
struct A;   // declaration 2 (ok)
struct B {  // declaration and definition
    int b;
    //A x;  // compile error incomplete type
    A* y;   // ok, pointer to incomplete type
};

struct A {  // definition
    char c;
}
```
<br></br>
# 열거자(Enumerator)
- 이름이 지어진 정수들의 그룹
```
enum color_t { BLACK, BLUE, GREEN };
color_t color = BLACK;          //int: 0
cout << (color == BLUE);        //print false

enum fruit_t { APPLE, CHERRY };
fruit_t fruit = APPLE;          //int: 0
bool b = (color == fruit);      //print 'true'!!
```
# Enum Class
- 암묵적으로 정수로 변환할 수 없는 안전한 타입의 열거형
```
enum class Color { BLACK, BLUE, GREEN };
enum class Fruit { APPLE, CHERRY };
Color color = Color::BLUE;
Fruit fruit = Fruit::APPLE;
//bool b = (color == fruit) compile error we are trying to match colors with fruits
//BUT, they are different things entirely
//int a1 = Color::GREEN; compile error
//int a2 = Color::RED + Color::GREEN; compile error
int a3 = (int) Color::GREEN; //ok, explicit conversion
```
# 특징
- 비교 가능
- 증가하는 순서로 자동으로 열거
- 값을 지정할 수 있음
- 기본 유형을 설정할 수 있음(C++11)
- 직접 목록 초기화 지원(C++17)
    ```
    enum class Color { RED, GREEN, BLUE };
    Color a{2};                 // ok, equal to Color:BLUE
    ```
- 특성 지원
    ```
    enum class Color { RED, GREEN, BLUE [[deprecated]] };
    auto x = Color::BLUE;       // compiler warning
    ```
- 로컬 스코프에 열거형 식별자 선언 가능(C++20)
    ```
    enum class Color { RED, GREEN, BLUE };
    switch (x) {
        using enum Color;
        case RED:
        case GREEN:
        case BLUE:
    }
    ```
# 일반적인 오류
- 항상 초기화해야 함
- `enum`/`enum class`의 기본 타입의 범위 밖의 값을 캐스팅하면 예기치 못환 동작이 발생(C++17)
    ```
    enum Color : uint8_t { RED, GREEN, BLUE };
    Color value = 256;          // undefined behavior
    ```
#enum/enum class와 constexpr
- `constexpr`식은 열거에 대한 범위를 벗어난 값을 허용하지 않음
    ```
    enum Color { RED };
    enum Fruit : int { APPLE };
    enum class Device { PC };
    //constexpr Color a1 = (Color) -1; compile error
    const Color a2 = (Color) -1;        // ok
    constexpr Fruit a3 = (Fruit) -1;    // ok
    constexpr Device a4 = (Device) -1;  // ok
    ```
## const vs constexpr
- `const`는 컴파일, 런타임 시간에 상수를 결정되는 값을 가질 수 있는 반면 `constexpr`은 컴파일 시간에 상수를 결정되는 값을 가짐
- `const`와 관련된 오류는 런타임에 발생하는 반면 `constexpr`은 컴파일 시간에 발생
## 자료
- https://github.com/federico-busato/Modern-CPP-Programming/blob/master/04.Basic_Concepts_III.pdf
- https://stackoverflow.com/questions/41125651/constexpr-vs-static-const-which-one-to-prefer
