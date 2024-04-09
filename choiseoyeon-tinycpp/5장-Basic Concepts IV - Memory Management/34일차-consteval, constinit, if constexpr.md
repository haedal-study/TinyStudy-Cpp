# consteval 키워드
- consteval 혹은 즉석 함수는 해당 함수의 평가를 컴파일 타임에 할 수 있도록 보장해준다.
- const가 아닌 값들은 항상 컴플래이션 에러를 뱉는다.
```
consteval int square(int value)
{
	return value * value;
}

square(4); // 컴파일 단계에서 평가됨

int v = 4; // v는 동적이다.
// square(v); // 컴파일 러러
```
# constinit 키워드
- constinit 는 변수에 대한 컴파일 타임의 초기화를 보장한다.
- const가 아닌 값들은 항상 컴플레이션 에러를 뱉는다.
- 평가중 변수의 값은 변경될 수 있다.
- `const constinit` 은 `constexpr`을 의미하지 않는다.(반대는 맞음)
- `constexpr`은 그 수명 동안 컴파일 타임 동안의 평가를 필요로 한다.
```
constexpr int square(int value) {
	reutrn value * value;
}
constinit int v1 = square(4); // 컴파일 타임에 평가됨
v1 = 3; // v1는 변경가능

int a = 4; // a는 동적
// constinit int v2 = suare(a); // 컴파일 러러
```
# if constexpr
- C++ 17 `if constexpr` 기능은 컴파일 타임의 값에 따라 조건적인 컴파일 코드를 허용한다.
- `if constexpr` 문은 해당 브렌치를 컴파일 타임에 평가할 수 있도록 컴파일러에게 강제 한다.
	- `#if` 전처리기와 비슷
```
auto f() {
	if constexpr (sizeof(void*) == 8)
		return "hello"; // const char*
	else
		return 3; //int, 컴파일 되지 않음
}
```
- 예시
```
constexpr int fib(int n) {
	return (n == 0 || n == 1) ? 1 : fib(n - 1) + fib(n - 2);
}

int main() {
	if constexpr (sizeof(void*) == 8)
		return fib(5);
	else
		return fib(3);
}
```
- 함정(실수하기 쉬운 것)
```
// if constexpr 은 if/else 문에서만 사용 가능하다
auto f1() {
	if constexpr (my_constexpr_fun() == 1)
		return 1;
	// return 2.0; // 컴파일 에러, constexpr의 부분이 아님
}

// else if 브랜치는 constexpr이 요구된다.
auto f2() {
	if constexpr(my_constexpr_fun() == 1)
		return 1;
	else if(my_constexpr_fun() == 2) // else if constexpr 라고 적어야함
		// return 2.0; // 컴파일 에러, constexpr의 부분이 아님
	else
		return 3L;
}
```