## 출력 방식
- C
	- printf함수를 사용해 출력
```
#include <stdio.h>

int main() {

printf("Hello World!\n");

}
```
- C++
	- cout 표준 출력 스트림을 사용해서 출력
```
#include <iostream>

int main() {

std::cout << "Hello World!\n";

}
```
---
## I/O Stream
- std::cout은 C++의 표준 출력 스트림
- 출력 스트림이란 데이터를 출력하는 일련의 흐름
---
## I/O Stream을 선호하는 이유
- 타입 안정성
	- 사용자가 변수의 유형을 지정할 수 있음
	- 컴파일 타임에 유형 불일치 검사를 할 수 있음
	- printf와 같은 함수는 서식 지정자(% fields)를 사용하여 변수 유형을 동적으로 추론. 잘못된 서식 지정자를 사용할 위험이 있음
- 오류 발생 가능성 적음
	- I/O Stream은 객체를 출력하는데 직접 사용되므로 서식지정자와 객체간의 일관성을 유지할 필요가 없음
- 확장성
	- 사용자가 정의한 새로운 데이터 유형을 만들어서 해당 객체를 입출력 스트림에 전달할 수 있음
- 성능
	- C의 입출력 함수인 printf, scanf등과 비교할만한 성능을 가지고 있음
--- 
## std::printf
- C++23버전에서 개선된 버전의 printf함수인 std::printf가 도입되었음
- C++ 스트림의 이점을 제공하면서도 코드를 덜 작성할 수 있음
```
#include <print>

int main() {

std::print("Hello World! {}, {}, {}\n", 3, 4ll, "aa");

// "Hello World! 3 4 aa"

}
```
