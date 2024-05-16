
### 고정 너비 정수(Fixed width integer)
---

* C++에서는 정수 자료형들이 모든 아키텍쳐에서 같은 사이즈를 유지하도록 고정된 사이즈의 정수를 제공한다.

* **종류**
	int8_t, uint8_t
	int16_t, uint16_t
	int32_t, uint32_t
	int64_t, uint64_t

* **주의사항**

	* 위 고정 너비 정수는 실제 **타입이 아니다**. 적절한 기본 자료형을 typedef 하는 것과 같다.
	
	* 고정 너비 정수가 기본 자료형들을 1대 1로 매핑하는 것은 아니다.
	
	* I/O Stream에서는 uint8_t와 int8_t를 **char**로 해석한다. (int8_t라고 해서 int로 받아들이고 계산하면 잘못될 수도 있다.)

```cpp
int8_t var;
cin >> var;   // read '2'
cout << var;  // print '2'
int a = var * 2;
cout << a     // print '100'
```


### size_t and ptrdiff_t
---

* size_t와 ptrdiff_t는 현재 아키텍쳐에서 표현 가능한 가장 큰 값이 저장될 수 있는 타입의 별칭이다.

* < cstddef >에서 사용할 수 있다.

* 32-bit 아키텍쳐에서는 부호 없는 32-bit 정수, 64-bit 아키텍쳐에서는 부호 없는 64-bit 정수이다.

* size_t
	* size_t는 부호 없는 정수 타입의 별칭이다. (최소 16 bit 이상)
	* 보통 어떤 것의 크기를 나타내기 위해 사용된다.

* utrdiff_t
	* ptrdiff_T는 size_t의 signed 버전이다. 
	* 주로 포인터 차이를 계산하는데 쓰인다.
