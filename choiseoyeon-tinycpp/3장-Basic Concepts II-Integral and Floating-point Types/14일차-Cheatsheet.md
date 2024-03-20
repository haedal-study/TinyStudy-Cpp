- 치트 시트
![[Pasted image 20240320112001.png]]
- 부동소수점의 형태
![[Pasted image 20240320112056.png]]![[Pasted image 20240320112107.png]]
- 부동소수점의 한계
```
#include <limits>
std::numeric_limits<T>::max();        // 최대값
std::numeric_limits<T>::lowest();     // 가장 낮은 값
std::numeric_limits<T>::min();        // 최소값
std::numeric_limits<T>::denom_min();  // 비정규 값의 최대값
std::numeric_limits<T>::epsilon();    // 입실론 값
std::numeric_limits<T>::infinity();   // 무한
std::numeric_limits<T>::quiet_NaN();  // NaN
```
- 쓸만한 함수들
```
#include <cmath>

bool std::isnan(T value) // NaN 값인지 판별
bool std::isinf(T value) // (+-) 무한 값인지 판별
bool std::isfinite(T value) // 무한값도, NaN 값도 아닌 값인지 판별


bool std::isnormal(T value) // 보통 값인지 판별

T    std::ldexp(T x, p) // x * 2의 p승 값을 반환
int  std::ilogb(T value) // 지수 값을 반

```