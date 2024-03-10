## 기본적인 C++ 입출력 문법
```
#include <iostream>

int main() {
	std::cout << "Hello World!\n";
}
```
#### 1) C언어의 printf 문과 C++의 cout 의 차이
###### printf
- 기본적인 output을 의미
###### cout
- 기본적인 output 스트림을 대표함
#### 2) `std` 네임스페이스
위의 코드는 다음과 같이 std 네임스페이스와 함께 사용될 수 있다.
```
#include <iostream>

using namespace std;
int main() {
	cout << "Hello World!\n";
}
```
----
## I/O 스트림

std::cout은 output 스트림의 예시이다. `<<` 을 통해 데이터는 기본적인 output 스트림으로 이어진다.

다음은 동일한 코드의 예시이다.
```
// C
#include <stdio.h>
int main()
{
	int a = 4;
	double b = 3.0;
	char c[] = "hello";
	printf("%d %f %s\n", a, b, c);
}
```

```
//C++
#include <iostream>
int main()
{
	int a = 4;
	double b = 3.0;
	char c[] = "hello";
	std::cout << a << " " << b << " " << c << "\n";
}
```

#### I/O 스트림을 선호해야하는 이유
###### 타입이 안정적임
- I/O 스트림에 제공 되는 타입들은 컴파일러에 의해 정적으로 알게 된다.
- 반대로 `printf` 는 `%`을 사용하여 동적으로 타입을 알아내야 한다.
###### 에러가 덜 발생함
- I/O을 사용하면 불팔요한 `%` 이 전달되지 않기 때문에, 에러가 적다.
###### 확장하기 편함
- C++ I/O 스트림 메커니즘은 사용자가 지정한 타입들이 기존의 코드의 수정 없어 I/O 스트림을 지나갈 수 있도록 해준다.
###### 성능이 더 좋음
- 정확하게 쓴다면 C언어의 입출력보다 더 빠를 수 있다.
----
## std::printf

C++23 버전에서 C++의 스트림의 이점을 제공하면서 코드를 덜 쓸 수 있는 `std::printf` 기능이 추가되었다.
```
#include <print>

int main() {
	std::print("Hello World! {}, {}, {}\n", 3, 4ll, "aa");
	// Hello World! 3 4 aa
}
```
C++ 23 버전이 보편화 될 경우 이것이 기본적인 출력 방식일 것이다.