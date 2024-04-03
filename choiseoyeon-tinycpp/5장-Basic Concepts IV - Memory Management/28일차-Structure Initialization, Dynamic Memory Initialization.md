## C++03에서의 구조체 초기화
```
struct S {
	unsigned x;
	unsigned y;
};

S s1; // 기본 초기화, xy는 알 수 없는 값
S s2 = {}; // 복사 초기화, xy는 0(디폴트) 값으로 초기화
S s3 = { 1, 2 }; // 복사 리스트 초기화
S s4 = { 1 }; // 복사 리스트 초기화, x = 1 y는 디폴트 값
// S s5(3, 5); // 컴파일 에러, 생성자 없음

S f() {
	S s6 = { 1, 2 };
	return s6;
}
```
## C++11에서의 구조체 초기화
```
struct S {
	unsigned x;
	unsigned y;
	void* ptr;
};

S s1 {}; // 직접 리스트(값) 초기화, x, y ptr은 0(디폴트)값으로 초기화

S s2{1, 2}; // 직접 리스트(값) 초기화, x = 1, y = 2, ptr은 0(디폴트) 값으로 초기화

// S s3{1, -2};// 컴파일 에러, 값의 범위 넘음

S f() { return {3, 2}; } //
```
## 연쇄 혹은 동일 초기화
- 비정적 데이터 멤버 초기화
```
struct S {
	unsigned x = 3; // 동일 초기화
	unsigned y = 2; // 동일 초기화
};
struct S1 {
	unsigned x {3}; // 연쇄 초기화
};

S s1; // 기본 생성자 호출
S s2 {} ;// 기본 생성자 호출
S s3{1, 4}; // x=1, y=4
```
## 지정 초기화 리스트
- C++20 에서는 지정 초기화 리스트를 제공함
```
struct A {
	int x, y, z;
};
A a1 { 1,2,3}; 
A a2 { .x = 1, .y = 2, .z = 3};// 위와 동일, 지정 초기화 리스트
```
- 지정 초기화 리스트는 코드의 가독성을 높혀줌
```
void f1(bool a, bool b, bool c, bool d, bool e) {}
// 같은 데이터 타입의 많은 매개변수 >> 에러 발생 요인!

struct B { 
	bool a, b, c, d, e;
}; // f2(B b)가 있다면
f2({.a = true, .c = true}) // b,d,e 는 false 값 
```
## 구조체 바인딩
- C++17에서는 초기화에서 특정한 이름의 요소를 바인딩 해준다.
```
struct A {
	int x = 1;
	int y = 2;
} a;

A f() { return A{4, 5}; }
// struct
auto [x1, y1] = a; // x1 = 1, y1 = 2
auto [x2, y2] = f(); // x2 = 4, y2 = 5
// 기본 배열
int b[2] = {1,2};
auto [x3, y4] = b; // x3 = 1, y4 = 2
// 튜플
auto [x4, y4] = std::tuple<float, int>{3.0f, 2};
```
## 동적 메모리 초기화
- C++03
```
int* a1 = new int; //정의되지 않은 값
int* a2 = new int(); // 디폴트 초기화 = int()를 호출함
int* a3 = new int(4); //4에 해당하는 하나의 값을 할당
int* a4 = new int[4]; // 4개의 정의되지 않는 값 할당
int* a5 = new int[4](); // 디폴트 값의 4개의 요소에 값 할당
//int* a6 = new int[4](3); // 불가능함
```
- C++11
```
int* b1 = new int[4]{};// 초기화된 4개의 값을 할당
int* b2 = new int[4]{1, 2}; // 첫번째 두번째 값만 세팅, 나머지는 초기화 값
```