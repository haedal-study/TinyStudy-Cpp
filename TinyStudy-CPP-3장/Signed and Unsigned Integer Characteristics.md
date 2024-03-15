
* **Overflow** : 산술 연산이 단어의 길이(양/음의 최대값)를 초과해서 생기는 결과.
* **Wraparound** : 산술 연산이 단어의 길이를 초과해 2^N(N은 단어의 비트 수)로 모듈로 연산을 수행하는 것.

### Signed Integer
---

* 음수, 0, 양수 값을 가짐.
* Overlow/Underflow 의미론 --> 정의되지 않은 행동
* Bit-wise 연산은 구현에 따라 정의. 

### Unsigned Integer
---

* 음이 아닌 정수를 가짐.
* Wraparound 의미론 --> 정의됨(모듈로 연산)
* Bit-wise 연산이 정의되어 있음.


### Signed, Unsigned는 언제 써야할까?
---

* Signed integer
	* 음수를 써야 할 경우.
	* 음이 아닌 값을 부호 있는 정수로 표현하고 싶은 경우.
	* 최적화 목적. (ex. 루프에서 오버플로우를 활용할 때)

* Unsigned integer
	* 음수가 될 일이 없는 값일 경우.
	* 비트마스크 값일 경우.
	* 최적화 목적. (ex. 나눗셈, 모듈로)
	* 안정적인 시스템을 원할 경우.