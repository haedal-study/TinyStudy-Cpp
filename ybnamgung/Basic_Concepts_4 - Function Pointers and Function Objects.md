# Basic_Concepts_4 - Function Pointers and Function Objects

## Function Pointer

표준 C는 함수 포인터의 개념을 통해 일반 프로그래밍 능력과 조립성을 실현한다

함수는 다른 함수에 대한 포인터로 전달될 수 있으며 '간접 호출(indirect call)'로 작동한다

```cpp
#include <stdlib.h>

int descending(const void* a, const void* b) {
    return *((const int*)a) > *((const int*)b);
}

int array[] = { 7, 2, 5, 1 };

qsort(array, 4, sizeof(int), descending);
// array : { 1, 2, 5, 7 }
```

```cpp
int eval(int a, int b, int (*f)(int, int)) {
    return f(a, b);
}

int add(int a, int b) { return a + b; }
int sub(int a, int b) { return a - b; }

cout << eval(4, 3, add) << endl; // 7
cout << eval(4, 3, sub) << endl; // 1
```

문제점

- 안전성 : 제네릭의 경우(예: qsort)에서는 인자의 타입 검사가 없다.
- 성능 : 어떤 연산이든 원래 함수에 대한 간접 호출이 필요하다. 함수 인라이닝은 불가능하다.

## Function Object

Function Object 또는 Functor는 호출 가능한 객체로, 매개변수처럼 취급될 수 있다.

C++는 다른 함수에 프로시저를 전달하는 데 더 효율적이고 편리한 방법을 제공한다. 이를 Function Object라고 한다.

```cpp
#include <algorithm>

using namespace std;

struct Descending { // function object
    bool operator()(int a, int b) { // 함수가 연산자를 호출한다.
        return a > b;
    }
};

int array[] = { 7, 2, 5, 1 };

sort(array, array + 4, Descending{});
// array: { 7, 5, 2, 1 }
```

장점

- 안정성 : 인자 타입 검사가 항상 가능하다. 템플릿을 사용할 수 있다.
- 성능 : 컴파일러는 대상 함수의 코드에 operator()를 삽입하고 루틴을 컴파일한다. 연산자 인라이닝이 기본 행동이다.
- C++11은 람다 표현식이라고 불리는 덜 번잡한 함수 객체를 제공함으로써 이 개념을 단순화한다.