# Template parameters - Default Value
---
```cpp
template<int A = 3, int B = 4>
void print1() { cout << A << ", " << B; }

template<int A = 3, int B> // 가능하지만, 의미가 없다.
void print2() { cout << A << ", " << B; }

print1<2, 5>(); // print 2, 5
print1<2>(); // print 2, 5
print1<>(); // print 3, 4
print1(); // print 3, 4

print2<2, 5>(); // print 2, 5
// print2<2>(); // error
// print2<>(); // error
// print2(); // error
```

템플릿 매개변수에 이름이 없을 수 있다.
```cpp
void f() {}

template<typename = void>
void g() {}

int main() {
	g(); // generated
}
```

`f()`는 언제나 최종 코드에 생성된다.
`g()`는 호출했을때만 최종 코드에 생성된다.

함수 매개변수와 달리 템플릿 매개변수는 이전 매개변수 값으로 초기화할 수 있다.
```cpp
template<int A, int B = A + 3>
void f() {
	cout << B;
}

template<typename T, int S = sizeof(T)>
void g(T) {
	cout << S;
}

f<3>(); // B is 6
g(3); // S is 4
```
# Overloading
---
템플릿 함수는 오버로드 될 수 있다.
```cpp
template<typename T>
T add(T a, T b) {
	return a + b;
} // e.g add(3, 4);

template<typename T>
T add(T a, T b, T c) { // 매개변수의 개수가 다름
	return a + b + c;
} // e.g add(3, 4, 5);
```

또한, 템플릿 자체도 오버로드 될 수 있다.
```cpp
template<int C, typename T>
T add(T a, T b) { // T add(T a, T b)와 충돌하지 않는다. "C"도 시그니처의 일부이다.
	return a + b + C;
}
```

# Specialization
---
템플릿 특수화는 템플릿 매개변수의 특정 조합에 대한 구체적인 구현을 의미한다.

```cpp
template<typename T>
bool compare(T a, T b) {
	return a < b;
}
```
위 코드에서 부동소수점을 사용할 경우 반올림 오류로 인해 부정확할 위험이 있다.

```cpp
template<>
bool compare<float>(float a, float b) {
	return ... // 더 나은 부동소수점 구현
}
```

전체 특수화 : 모든 템플릿 인자가 전문화된 경우에만 함수 전문화를 할 수 있다.