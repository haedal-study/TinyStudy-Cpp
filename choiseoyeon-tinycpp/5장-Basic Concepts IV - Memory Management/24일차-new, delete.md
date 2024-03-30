- `new`/`new[]` 와 `delete`/`delete[]` 는 동적 메모리 할당 및 반환, 그리고 런타임 중 오브젝트 생성 및 삭제를 담당하는 C++의 키워드이다.
	- `malloc`과 `free`는 C 언어의 기능이며, 그들은 그냥 메모리를 할당, 반환만 가능하다.
## new, delete의 장점
- 키워드임: 함수가 아니기에 안전함
- 반환 타입이 있음: new는 해당 데이터 타입을 반환한다
	- malloc은 void*를 반환
- 예외 처리: new는 예외를 던진다.
	- malloc은 NULL 포인터를 반환함
- 공간 할당: malloc은 사용자가 크기를 계산해야 하지만, new 키워드는 컴파일러가 이를 계산해준다.
- 초기화: new 키워드는 할당 말고도 초기화를 시켜준다.
- 다형성: 가상 함수를 갖고 있는 오브젝트는 new 키워드로 할당되어야 한다.
	- v-테이블을 초기화 하기 위해
## 동적 메모리 할당
```
// 단일 객체 할당
int* value = (int*) malloc(sizeof(int)); // C
int* value = new int; // C++

// N개의 객체 할당
int* array = (int*) malloc(N * sizeof(int)); // C
int* array = new int[N];

// N개의 구조체 할당
MStruct* array = (MStruct*) malloc(N * sizeof(MStruct)); // C
MStruct* array = new MStruct[N];

// N개의 객체 할당 및 제로 초기화
int* array = (int*) calloc(N, sizeof(int)); //C
int* array = new int[N] (); // C++

// 단일 객체 해제
int* value = (int*) malloc(sizeof(int)); //c
free (value);

int* value = new int; // c++
delete value;

// N개의 객체 해제
int* value = (int*) malloc(N * sizeof(int)); //C
free(value);

int* value = new int[N]; // C++
delete[] value;
```
## 기본 규칙
- `malloc`으로 할당했으면 `free()`로 해제 해야함
- `new`로 할당했으면 `delete`로 해제 해야함
- `new[]`로 할당했으면 `delete[]`로 해제 해야함
- `malloc()` `new` `new[]`를 성공 시에는 null 포인터를 절대로 생상하지 않는다.
	- (정의에 따라다르지만) 크기를 0으로 했을 경우 제외
- `free()` `delete` `delete[]`에 nullptr이나 NULL 포인터를 넣을 경우 에러가 발생된다.

- `new`와 `new[]`, `malloc`를 다른 것과 함께 사용하면 정의 되지 않는 행동을 일으킬 수 있다.
## 2차원 메모리 할당
```
// 2차원 동적 메모리 할당
int** A = new int*[3];
for(int i = 0; i < 3; i++)
	A[i] = new int[4];

for(int i = 0; i < 3; i++)
	delete[] A[i];
delete[] A;

// C++11에서의 2차원 동적 메모리 할당
auto A = new int[3][4]; // int[4] 타입의 오브젝트를 3개 할당

int n = 3; // 동적으로 변할 수 있는 값
auto B = new int[n][4]; // 가능함
// auto C = new int[n][n]; // 컴파일 에러 발생

delete[] = A; // 위의 모든 상황에서 사용 가
```