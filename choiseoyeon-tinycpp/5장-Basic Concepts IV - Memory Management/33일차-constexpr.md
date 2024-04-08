- `constexpr` 지정자는 해당 구문이 컴파일 타임에 평 될 수 있도록 해준다.
- `const` 는 프로그램이 돌아가는 동안 변수의 값이 고정시킨다.
- `constexpr`는 `const`를 암시한다.
- `constexpr`은 퍼포먼스와 메모리 사용량에 도움을 준다.
- `constexpr`은 컴파일 시간에 영향을 줄 수도 있다.
- `constexpr` 변수들은 항상 컴파일 타임에 평가된다.
```
const int v1 = 3; // 컴파일 타임에 평가
const int v2 = v1 * 2; // 컴파일 타임에 평가

int a = 3; // a는 동적이다.
const int v3 = a; // 런타임에 평가됨

constexpr int c1 = v1; // 가능함
// constexpr int c2 = v3; // 컴파일 에러, v3은 동적임
```
- constexpr 함수
	- constexpr 함수는 그 내용이 모두 컴파일 타임에 평가 된다면, 해당 함수의 평가가 컴파일 타임에 이루어 지는 것을 보장해준다.
- 런타임 함수, 즉 constexpr가 아닌 함수를 포함해서는 안된다.
- C++11: 하나의 return 문만 포함이 가능하며, 루프나 스위치문을 포함해서는 안된다.
- C++14: 제한 없음
```
constexpr int square(int value)
{
	return value * value;
}
square(4); // 컴파일 타임에 평가
int a = 4; // a는 동적임
square(a); // 런타임에 평가
```
- constexpr 키워드의 제한 사항
	- try-catch 문, 예외, RTTI와 같은 런타임 기능을 포함해서는 안된다.
	- goto 문이나 asm 문을 포함해서는 안된다.
	- static  변수를 포함해서는 안된다.
	- C++14 버전이전에는 assert()를 사용해서는 안된다.
	- C++20 버전 이전까지는 virtual을 사용해서는 안된다.
	- 정의되지 않는 코드 또한 사용 불가능하다
		- reinterpert_cast, union의 안전하지 않는 사용, 부호 있는 정수 타입의 오버플로우 등
	- 런타임 오브젝트의 constexpr이면서 정적이지 않는 멤버 함수는 내부 동작이 모두 조건에 부합해도 사용할 수 없다.
	- `static constexpr` 멤버 함수는 어떤 특정한 인스턴스에 의존하지 않기 때문에 위의 조건에 해당하지 않는다.
	```
	struct A {
		constexpr int f() const {return 3;}
		static constexpr int g() {return 4;}
	};

	A a1;
	// constexpr int x = a1.f(); // 컴파일 에러
	constexpr int y = a1.g(); // 가능, A::g()도 가능

	constexpr A a2;
	constexpr int x = a2.f(); // 가능
	```