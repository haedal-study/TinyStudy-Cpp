# Basic_Concepts_3 - sizeof Operator

## sizeof

`sizeof`는 컴파일 타임에 변수나 데이터 타입의 크기를 바이트 단위로 결정하는 연산자이다.

- 반환타입은 `size_t`이다.
- 절대로 0을 반환하지 않는다.
- 구조체의 크기를 구할땐 언제나 내부 패딩도 고려하여 크기를 결정한다.
- 참조의 크기를 구할땐 참조된 타입의 크기를 반환한다.
- 불완전한 타입을 대상으로 하면 컴파일 에러가 발생한다 예 : void
- bitfield 멤버를 대상으로 하면 컴파일 에러가 발생한다.

```cpp
void temp(int tmp[]) {
    cout << sizeof(tmp);
}

int array1[10];
int* array2 = new int[10];
cout << sizeof(array1) << endl; // 40
cout << sizeof(array2) << endl; // 4 (32-bit)
temp(array1); // 4 (32-bit)
```

```cpp
struct A {
    int x; // offset 0
    char y; // offset 4
};
sizeof(A); // 8 바이트 : 4 + 1 (+ 3 패딩)

struct B {
    int x; // offset 0
    char y; // offset 4
    short z; // offset 6
};
sizeof(B); // 8 바이트 : 4 + 1 (+ 1 패딩) + 2

struct C {
    short z;
    int x;
    char y;
};
sizeof(C); // 12 바이트 : 2 + (+ 2 패딩) + 4 + 1 (+ 3 패딩)
```

```cpp
char a;
char& b = a;
sizeof(&a); // 4바이트 (32-bit) (포인터의 크기)
sizeof(b); // 1바이트 (char의 크기)

struct A {};
sizeof(A); // 1 (절대 0을 반환하지 않는다.)

A array1[10];
sizeof(array1); // 10

int array2[0]; // gcc에서만 가능하다.
sizeof(array2); // 0 : 특이 케이스
```