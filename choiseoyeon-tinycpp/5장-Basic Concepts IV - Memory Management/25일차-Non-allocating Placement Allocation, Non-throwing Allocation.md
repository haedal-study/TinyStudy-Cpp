- 비할당 배치는 이전에 할당 된 각각의 오브젝트의 메모리 주소를 명시적으로 지정할 수 있도록 한다.
```
// 스택메모리에서
char buffer[8];
int* x = new (buffer) int;
short* y = new (x + 1) short[2];
// x와 y는 해제를 해줄 필요가 없다.
```
```
// 힙 메모리에서
unsigned* buffer2 = new unsigned[2];
double* z = new (buffer2) double;
delete[] buffer2; // 가능함
// delete[] z; // 가능은 하지만 좋지 않음
```
- (런타임 중에 명시적으로 오브젝트의 소멸자를 불러야하는)사소하지 않는 오브젝트의 공간 할당은 오브젝트가 스코프를 벗어났을 때 확인할 수 없다.
```
struct A {
	~A() { cout << "destructor"; }
};

char buffer[10];
auto x = new (buffer) A();
// delete x; // 런타임 에러 발생, x는 유효한 힙 메모리 포인터가 아님
x->~A(); // destructor 출
```
- `new` 연산자는 `std::nothrow` 오브젝트를 통해 비할당 배치를 허용할 수 있다.
- 이것은 메모리 할당이 실패하지 않는 이상 `std::bad_alloc`이 아닌 `NULL` 포인터를 반환한다.
```
int* array = new (std::nothrow) int[very_large_size];
```
> `new` 할당하는 크기가 0이어도 NULL 포인터를 반환한다.

`std::nothrow`는 할당된 오브젝트가 예외를 발생할 수 없다는 뜻이 아니다.
```
struct A {
	A() { throw std::runtime_error{}; }
};
A* array = new (std::nothrow) A;
```