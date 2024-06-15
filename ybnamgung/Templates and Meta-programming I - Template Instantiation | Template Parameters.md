# Template Instantiation
템플릿 인스턴스화는 템플릿 파라미터를 구체적인 값이나 타입으로 대체하는 과정이다.
컴파일러는 각 템플릿 인스턴스화에 대해 함수 구현을 자동으로 생성합니다.

```cpp
template<typename T>
T add(T a, T b) {
	return a + b;
}
add(3, 4); // generates : int add(int, int)
add(3.0f, 4.0f); // generates : float add(float, float)
add(2, 6); // already generated
// other instances are not generated
// e.g. char add(char, char)
```

- 암시적 템플릿 인스턴스화 : 컴파일러가 함수 또는 클래스 템플릿이 실제로 필요할 때, 주어진 인자 타입에 따라 자동으로 템플릿을 인스턴스화 하는 방식
- 명시적 템플릿 인스턴스화 : 특정 타입을 명시적으로 지정하여 템플릿을 인스턴스화 하는 방식. 여러번의 인스턴스화를 피하고 바이너리 크기를 줄이기 위해 유용하다.

```cpp
template<typename T>
void f(T a) {}

void g() {
	f(3); // generates: void f(int) -> implicit
	f<short>(3.0f); // generates : void f(short) -> implicit
}

template void f<int>(int); // generates : void f(int) -> explicit
```
# Template Parameters
템플릿 매개변수는 `template`키워드 뒤에 오는 이름들이다.
```cpp
template<typename T>
void f() {}

f<int>();
```

`typename T`는 템플릿 매개변수이다.
`int`는 템플릿 인자이다.

템플릿 매개변수는 제네릭 타입일 수 있으며, 비타입 템플릿 매개변수(NTTP)일 수도 있다.int, enum 등이 있다.
제네릭 타입의 템플릿 인자는 내장 타입이거나 사용자가 선언한 타입일 수 있으며, 비타입 템플릿 매개변수의 경우 구체적인 값이 된다.

템플릿을 사용하면 컴파일 타임에 처리되기 때문에 성능을 최적화할 수 있으며, 비타입 템플릿과 제네릭 타입을 함께 사용하여 유연성과 성능의 이점을 동시에 가져올 수 있다.

```cpp
template<int A, int B>
int add_int() {
	return A + B; // 더하기는 컴파일 타임에 계산된다. e.g. add_int<3, 4>();
}
```

```cpp
enum class Enum { Left, Right };

template<Enum Z>
int add_enum(int a, int b) {
	return (Z == Enum::Left) ? a + b : a; // e.g. add_enum<Enum::Left>(3, 4);
}
```

- Ceiling division
```cpp
template<int DIV, typename T>
T ceil_div(T value) {
	return (value + DIV - 1) / DIV;
}
// e.g. ceil_div<5>(11); // return 3
```
- Rounded division
```cpp
template<int DIV, typename T>
T round_div(T value) {
	return (value + DIV / 2) / DIV;
}
// e.g. round_div<5>(11); // return 2 (2.2)
```

DIV값은 컴파일 타임에 알 수 있으며, 이를 통해 최적화할 수 있다.