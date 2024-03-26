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
## 자료
- https://github.com/federico-busato/Modern-CPP-Programming/blob/master/04.Basic_Concepts_III.pdf
