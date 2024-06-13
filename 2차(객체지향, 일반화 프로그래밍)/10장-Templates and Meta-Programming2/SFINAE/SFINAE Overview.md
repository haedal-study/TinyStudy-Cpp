
### SFINAE : Substitution Failure Is Not An Error
---

- SFINAE는 직역하면 "치환 실패는 오류가 아니다."이다.
- SFINAE는 오버로드할 함수를 컴파일러가 결정하는 과정에서 발생한다.
- 컴파일러가 템플릿 인자를 치환하는데 실패했을 때, 이를 오류를 발생키는 것이 아니라 오버로드 후보에서 해당 특수화를 제외시킨다는 뜻이다.

```cpp
int negate(int i) { return -i; }

template <typename T>
typename T::value_type negate(const T& t) {
	return -T(t);
}

cout << negate(42); // -42
```

- 위 코드에서 컴파일러는 첫번째 `negate`함수를 선택해 -42를 출력할 것이다.
- 그런데 두번째 후보인 `T::value_type negate(const T& t)` 또한 확인하게 되는데
- 여기서 치환이 이루어져 `int::value_type negate(const T& t)`가 된다.
- `int`는 `value_type`이라는 데이터를 갖지 않기 때문에 치환이 실패하고, `int`에 해당하는 템플릿 함수의 오버로드는 제외된다.


### `std::enable_if` Type Trait
---

- 사용자가 원하는 타입만을 안전하게 오버로드 하기 위해 함수 선언부에 `std::enable_if`를 사용해 타입 치환 오류를 발생시킬 수 있다.
- 아래와 같이 정의될 수 있다.
```cpp
template<bool Condition, typename T = void>
struct enable_if {
	// "type" : Condition이 False이면 type이 정의되지 않는다.
};

template<typename T>
struct enable_if<true, T> {
	using type = T; // Contidion이 true인 부분 특수화.
};
```
- 별칭으로 `enable_if_t`가 있다.

- 예시
```cpp
// Condition은 정수타입인지 확인하는 is_integral_v Type Trait을 사용.
// enable_if_t의 결과가 nullptr이면 컴파일 에러 발생.
template <typename T, typename std::enable_if_t<std::is_integral_v<T>, T>* = nullptr>
void do_stuff(T& t) {
	std::cout << "do_stuff integral\n";
	// 정수 타입들을 받는 함수 (int, char, unsigned, etc.)
}

int main()
{
	int a = 10;
	do_stuff(a);
	float b = 12.2f;
	// do_stuff(b); // compile error: 
}
```


참고자료
[씹어먹는 C ++ 토막글 3 - SFINAE 와 enable_if (modoocode.com)](https://modoocode.com/255)
