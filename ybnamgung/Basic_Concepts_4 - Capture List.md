# Basic_Concepts_4 - Capture List

람다 표현식은 람다의 본문에서 사용되는 외부 변수를 두 가지 방법으로 캡처한다

- 값에 의한 캡처
- 참조에 의한 캡처 (외부 변수의 값을 수정할 수 있다)

캡처 리스트는 다음과 같이 전달될 수 있다.

- `[]` 캡처 없음
- `[=]` 모든 변수를 값에 의해 캡처
- `[&]` 모든 변수를 참조에 의해 캡처
- `[var1]` var1만 값을 통해 캡처
- `[&var2]` var2만 참조를 통해 캡처
- `[var1, &var2]` var1을 값에 의해, var2를 참조에 의해 캡처

```cpp
int limit = 5;

auto lambda1 = [=](int value) { return value > limit; }; // 값
auto lambda2 = [&](int value) { return value > limit; }; // 참조
auto lambda3 = [limit](int value) { return value > limit; }; // 값
auto lambda4 = [&limit](int value) { return value > limit; }; // 참조
// auto lambda5 = [](int value) { return value > limit; }; // 컴파일 에러. 캡처하고있음.

limit = 3;

int array[] = { 5, 2, 7, 1 };
int val = *find_if(array, array + 4, lambda4);
// lambda1, 3은 7 반환
// lambda2, 4는 5 반환
```

- `[=, &var1]` 모든 변수를 값에 의해 캡처하되, **`var1`**만 참조에 의해 캡처
- `[&, var1]` 모든 변수를 참조에 의해 캡처하되, **`var1`**만 값에 의해 캡처
- 변수가 constexpr로 선언되어 있으면 캡처하지 않고도 읽을 수 있다

```cpp
constexpr int limit = 5;
int var1 = 3, var2 = 4;

auto lambda1 = [](int value) { return value > limit; };
auto lambda2 = [=, &var2]() { return var1 > var2; };
```