# IEEE 부동 소수점
- IEEE754는 부동 소수점 연산의 기술 표준
- 일반적으로 C/C++는 IEEE754 부동 소수점 표준을 채택
<br></br>
# 부동 소수점
|종류|부호|지수부|가수부|타입|
|---|---|---|---|---|
|단일 정밀도|1bit|8bit|23bit|`float`|
|2배 정밀도|1bit|11bit|52bit|`double`|
|4배 정밀도|1bit|15bit|112bit|`std::float128`(C++23)|
|8배 정밀도|1bit|19bit|236bit|C++에 표준화되지 않음|
# 16비트 부동 소수점
|종류|부호|지수부|가수부|타입|사용|
|---|---|---|---|---|---|
|IEEE754|1bit|5bit|10bit|`std::binary16`(C++23)|GPU, Arm7|
|Google|1bit|8bit|7bit|`std::bfloat16`(C++23)|TPU, GPU, Arm8|
# 8비트 부동 소수점
- C++/IEEE에 표준화되지 않음

|종류|부호|지수부|가수부|
|---|---|---|---|
|E4M3|1bit|4bit|3bit|
|E5M2|1bit|5bit|2bit|
# 기타
- C++/IEEE에 표준화되지 않음
- TensorFloat-32(TF32)
    - 딥러닝 응용을 위한 특화된 부동 소수점 포맷
- Posit
    - 지수부와 가수부의 폭이 가변적인 부동 소수점
    - unum III(유니버설 넘버)라고 불림
- Microscaling Format(MX)
    - 저정밀 부동소수점 포맷
    -  FP8, FP6, FP4, (MX)INT8 포함
- Fixed-point
    - 소수점 뒤에 고정된 숫자
    - 인접한 숫자들 사이의 간격은 항상 같음
    - 부동 소수점 숫자에 비해 범위가 상당히 제한적
<br></br>
## 자료
- https://github.com/federico-busato/Modern-CPP-Programming/blob/master/03.Basic_Concepts_II.pdf

