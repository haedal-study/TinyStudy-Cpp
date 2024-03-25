# Enumerators
- `enum`은 이름이 지어진 정수 값의 그룹에 대한 데이터 타입이다.
```
enum color_t { BLACK, BLUE, GREEN };

color_t color = BLUE;
cout << (color == BLACK); // false 출력
```
### 문제
- 이넘의 종류는 다르나, 그 정수값이 같을 경우 true를 반환함

### enum class(C++11)
- `enum class`는 암묵적으로 int 타입으로 변형되지 않는 타입이 안전한 enumerator 데이터 타입이다.
- 이 경우 이넘의 종류가 다른 상태에서 비교 연산을 진행할 경우 컴파일 에러가 난다.
	- 또한 다른 이넘 클래스에 값을 대입하려고 하는 것 또한 컴파일 에러가 난다.
- 하지만 해당 타입을 명시적으로(static_cast 등) 형변환 시켜 int 타입에 대입할 경우는 가능하다.

### enum과 enum class의 기능들
- 비교 연산이 가능하다.
- 오름차순 형식으로 자동으로 값이 정해진다.
	```
	enum class Color { RED, GREEN = -1, BLUE, BLACK };
					   (0),  (-1),      (0), (1)
	// 이 경우 Color::RED == Color::BLUE 시 true가 나옴
	```
- 별칭을 넣을 수 있다.
	- 예를 들의 PC와 COMPUTER는 같은 의미이기 때문에 같은 값을 넣어 정의할 수 있다.
- C++11에서는 기본 데이터 타입을 지정할 수 있다(int8_t 등)
- C++11에서는 직접 목록 초기화가 가능하다
	- `a{2};`등
- C++17에서는 어트리뷰트를 사용할 수 있다.
- C++20에서는 enumerator 식별자를 로컬 스코프에서 사용할 수 있다.

### enum/enum class에서 실수하기 쉬운 것들
- 정의와 초기화와 함께 일어나야 한다.
- C++17에서는 기본 타입의 범위를 벗어난 값을 대입할 경우 정의되지 않는 연산으로 취급된다.

### enum/enum class과 constexpr
- C++17의  `constexpr` 표현식은 기본 타입이 없는 `enum`의 범위를 벗어난 값을 허용하지 않는다.