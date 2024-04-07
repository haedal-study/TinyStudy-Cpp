# Constants and Literals
- 상수는 컴파일 타임에 평가될 수 있는 문장이다.
- 리터럴은 상수에 할당할 수 있는 고정된 값이다.
	- "소스 코드에 내제 되어 있고, 상수 값을 표현하는  C++ 프로그램의 토큰이다."
- 리터럴 타입
	- 스칼라 타입인 bool, char, int, float, double 에 대한 구체적인 값
	- const char[] 타입의 스트링 리터럴
	- nullptr
	- 사용자 정의 리터럴
# const keyword
- `const` 키워드는 선언될 때 꼭 초기화 되어야 하며, 초기화 된 이후에는 절대로 값이 변경되지 않는 오브젝트를 가리킨다.
- `const` 변수는 오른쪽에 있는 구문이 컴파일 타임에 평가되면, 그 역시도 컴파일 타임에 평가된다.
```
int size = 3;
int A[size] = {1, 2, 3}; // 가능은 함. 그치만 C++ 기본에는 맞지 않음

const int SIZE = 3;
// SIZE = 4; // 컴파일 에러
int B[SIZE] = {1, 2, 3}; // 가능

const int size2 = size;
int C[size2] = {1, 2, 3}; // 좋지 않음! size2는 const가 아님.
```

## const keyword and Pointer
- `int*` -> `const int*`
- `const int*` -/> `int*`
```
void f1(const int* array) {} // array의 값들는 변경될 수 없음

void f2(int* array) {}

int* ptr = new int[3];
const int* cptr = new int[3];
f1(ptr); // 가능
f2(ptr); // 가능
f1(cptr); // 가능
// f2(cptr); // 불가능 컴파일 러러
```
- `int*` : int의 포인터
	- 포인터의 값은 변경가능 함
	- 포인터에 의해서 가르켜지는 요소 또한 변경 가능
- `const int*`: const int의 포인터. (const int)*
	- 포인터의 값은 변경 가능함
	- 포인터에 의해 가리켜지는 요소는 변경 불가
- `int *const`: int에 대한 상수 포인터
	- 포인터의 값은 변경 불가능
	- 포인터에 의해 가리켜지는 요소는 변경 가능
- `const int *const`: const int에 대한 상수 포인터
	- 포인터의 값은 변경 불가능
	- 포인터에 의해 가리켜지는 요소도 변경 가
- 포인터에 const를 넣는 것은 포인터 별칭에 const를 넣는 것과 다르다
```
using ptr_t = int*;
using const_ptr_t = const int*;

void f1(const int* ptr) {
	// ptr[0] = 0; //상수 오브젝트에 대한 포인터, 불가능
	ptr = nullptr; // 가능
}
void f3(const_ptr_t ptr) {
	// ptr[0] = 0; //상수 오브젝트에 대한 포인터, 불가능
	ptr = nullptr; // 가능
}
void f2(const ptr_t ptr) { // int* 에 대한 const
	ptr[0] = 0; // 가능!
	ptr = nullptr; // 불가능! 포인터의 값이 변경 가
}
```