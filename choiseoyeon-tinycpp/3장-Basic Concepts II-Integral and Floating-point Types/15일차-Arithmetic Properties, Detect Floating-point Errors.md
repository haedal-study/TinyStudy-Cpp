# Arithmetic Properties
- 부동소수점 연산에는 덧셈, 뺄셈, 곱셈, 나눗셈이 있다.
- 하지만 종종 정확한 값과는 오차를 나타낼 때가 있다.
	- 일반적으로 연산된 값이 정확한 값이 아님
	- 부동소수점의 등호 연산이 정확하지 않을 때가 있다.
	- 두 부동소수점의 순서를 다르게 연산하면, 그 결과 값은 같지 않다.
	- 여러 부동소수점의 연산에서 그 연산 순서가 달라지면 그 결과 값이 같지 않다.
	- (a+b) * c 는 (a *c) + (b * c)와 다르다.
	- (k / a) * a 는 k와 다르다
	- 부동 소수점은 포화 값인 inf, -inf 값이 있기에 언더 플로우와 오버플로우가 없다.
# Detect Floating-point Errors
`cfenv` 헤더에서 재고하는 함수를 통해 부동소수점 예외 상황이 발생했는지 확인 할 수 있다.(C++11)
```
#include <cfenv>
// 메크로
FE_DIVBYZERO    // 0으로 나눠졌는지
FE_INEXACT      // 반올림 에러
FE_INVALID      // NaN과 같은 인가 받지 않은 연산인지
FE_OVERFLOW     // 오버 플로우(inf에 도달했는지)
FE_UNDERFLOW    // 언더 플로우(-inf에 도달했는지)
FE_ALL_EXCEPT   // 모든 예외 

// 함수들
std::feclearexcept(FE_ALL_EXCEPT); // 예외 상태를 초기화
std::fetestexecept(<macro>); // 예외 상황이 발생되었다면 0이 아닌 값을 반환
```