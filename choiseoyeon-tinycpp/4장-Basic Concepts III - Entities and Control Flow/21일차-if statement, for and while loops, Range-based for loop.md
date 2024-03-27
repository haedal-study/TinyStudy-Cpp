# `if` statement
- `if` 문은 어떤 특정한 조건이 true로 평가될 때 첫번째 브렌치를 실행시키고, false일 때 두번째 브렌치를 실행 시킨다.
- 쇼트 서킷: OR 문에서 앞에 있는 값이 true이면 뒤에 있는 조건들을 검사하지 않고 무조건 실행 시킨다. 
# `for` and `while` Loops
- `for` 문은 얼마나 반복할지 알 수 있을 때 사용된다.
- `while` 문은 얼마나 반복할지 알 수 없을 때 사용된다.
- `do whlie` 문은 얼마나 반복할지는 모르나, 적어도 한번 실행되어야 할때 사용된다.
# `for` loop Features and Jump Statements
- C++에서는 루프 내에서의 정의를 허용한다.
- 무한 루프
```
for(;;) {}
while(true) {}
```
- 점프문
	- `break`: 루프를 종료시킴
	- `continue` : 다음 루프를 실행시킴
	- `return` : 함수에서 나가기
# Range-based for loop
- C++11에서는 범위 기반 for 루프문을 제공한다.
	- 이는 기존의 for 루프 구문을 간단하게 할 수 있게 해준다.
	- 이는 어떤 범위에서의 for 루프 작동과 동일하지만, 더 안전하게 사용할 수 있다.
- 범위 기반 for 루프 문은 유저가 시작과 끝과 증가폭을 지정하지 않도록 한다.
```
for (int v : {3, 2, 1})
	cout << v << " ";

int values[] = {3, 2, 1};
for (int v : values)
	cout << v << " ";

for (auto c : "abcd")
	cout << c << " "; // a b c d
```
- 범위 기반 for 루프는 3가지로 나뉠 수 있다.
	- 고정 크기 배열(ex. int array[3], "abcd")
	- 브렌치 초기화 리스트(ex. {1,2,3})
	- `begin()`과 `end()` 메소드를 갖고 있는 모든 오브젝트
- C++17에서는 구조체 바인딩으로 그 범위를 넓혔다.
```
struct A { int x; int y;};

A array[] = { {1,2}, {5,6}, {7,1} };
for(auto [x1, y1] : array)
	cout << x1 << "," << y1 << " "; //1,2 5,6 7,1
```