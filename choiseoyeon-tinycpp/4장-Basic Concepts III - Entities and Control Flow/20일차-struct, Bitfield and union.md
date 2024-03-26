# Struct
- `struct`는 다른 변수들을 하나의 유닛으로 모은다.
- `struct`의 정의 이후에 1개 혹은 그 이상의 변수를 선언할 수 있다.
- enumerator는 구조체 내에서 이름 없이 선언될 수 있다.
- 구조체는 (몇가지 제한 사항과 함께) 예를 들어 함수와 같은 로컬 스코프에서 선언될 수 있다.
# Anonymous and Unnamed struct
- 무명 구조체: 이름은 없지만 관련된 타입이 있는 것
- 익명 구조체: 이름과 타입이 없는 구조체
```
struct {
	int x;
} my_struct; // 무명 구조체

struct S {
	int x;
	struct { int y; }; // 익명 구조체
}
```
# Bitfield
- 비트필드는 구조체가 가질 수있는 변수로, 미리 지정된 크기를 갖고 있다. 비트 필드는 byte가 아니라 bit를 저장할 수 있다.
# union
- 유니온은 다른 데이터 타입을 같은 메모리 공간에 저장할 수 있는 특별한 데이터 타입이다.
- 유니온은 필드로 갖고 있는 데이터 멤버중 가장 큰 크기를 기준으로 크기를 갖는다.
- 유니온은 일종의 중복 저장소이다.
```
union A {
	int x;
	char y;
// x에 0xAABBCCDD가 저장되어 있다면
// y에는 0XDD가 저장된다(리틀 엔디안)
}
```

```
union A {
	int x;
	char y;
}; // sizeof(int)

A a;
a.x = 1023;
a.y = 0;
cout << a.x;
```
- 구조체와 다르게 C++은 익명 유니온을 허락한다.
- C++17에서는 타입이 안전한 union을 대표하는 `std::variant`를 사용할 수 있다.