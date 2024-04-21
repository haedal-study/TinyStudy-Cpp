# Basic_Concepts_3 - static_cast, const_cast, reintepret_cast

## static_cast, const_cast, reinterpret_cast

기존에 사용하던 형변환 스타일은 `(type) value`와 같은 형식이었다.

새로운 형변환 스타일은 다음과 같은 종류가 있다.

### static_case

`static_cast`는 컴파일 타임에 타입을 체크한다.

암시적, 사용자 정의 형변환을 사용한다.

```cpp
struct A {
    int temp;
};

struct B {
    int temp;
    B(const int& i)
    {
        temp = i;
    }
};

int a = 10;
float b = static_cast<float>(a);
// int* c = static_cast<int*>(a); // 컴파일 에러
// A c = static_cast<A>(a); // 컴파일 에러
B d = static_cast<B>(a); // 복사 생성자를 호출
```

### reinterpret_cast

`reinterpret_cast`는 `static_cast`와 비슷하지만, 형변환 연산자가 없어도 형변환이 가능하다는점에선 더 강력하다. `static_cast`와 달리 비트연산을 통해 형변환을 하기 때문에 의도치 않은 결과가 나올 수 있다.

```cpp
float a = 3.0f;
// 결과 : 3
int b = reinterpret_cast<int&>(b);
// 결과 : 1077936128
```

### const_cast

const_cast는 constness나 volatile을 제거할 수 있다.

```cpp
const int& a = 10;
const_cast<int&>(a) = 20; // Unknown behavior
```