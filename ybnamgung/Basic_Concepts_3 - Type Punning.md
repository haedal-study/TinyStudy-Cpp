# Basic_Concepts_3 - Type Punning

### Pointer Aliasing

서로 다른 포인터가 같은 메모리 공간을 가리킬때, 서로 동일한 메모리를 별칭(alias)한다는 의미이다.

### Type Punning

프로그래밍 언어의 형식 시스템을 우회하여 공식 언어의 범위 내에서는 어려우거나 불가능한 효과를 달성하기 위한 것

컴파일러는 strict aliasing rule이 절대로 위배되지 않는다고 가정한다. 만약 위배될경우 undefined behavior가 발생한다.

```cpp
// 최적화가 없이는 느리다.
float abs(float x) {
		return (x < 0.0f) ? -x : x;
}

// 직접 초기화되었다.
float abs(float x) {
		unsigned uvalue = reinterpret_cast<unsigned&>(x);
		unsigned tmp = uvalue & 0x7FFFFFFF; // 부호 비트를 0으로 초기화한다. 0은 양수이다.
		return reinterpret_cast<float&>(tmp);
}
// strict aliasing rule을 위배했으므로 undefined behavior이다.
```