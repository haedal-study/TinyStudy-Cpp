- 주소 연산자인 &는 변수의 주소를 반환한다.
```
int a = 3;
int* b = &a; // b는 a의 주소값과 같음

a++;
cout << *b; //4
```
- 레퍼런스 구문과 헛갈리지 않기...!

### 야생 포인터
```
int main() 
{
	int* ptr; // 이 포인터가 어떤 곳을 가리키는지 알 수 없음
	// 포인터를 초기화시자
}

// 댕글링 포인터
int main() {
	int* array = new int[10];
	delete[] array; //array는 댕글링 포인터
	delete[] array; // 두번 해제를 시도함! array는 여전히 null이 아님!
}

int* array = new int[10];
delete[] array; //array는 댕글링 포인터
array = nullptr; // 댕글링 포인터 아님
delete[] array; // 사이드 이펙트 없음. 가능
```
### void 포인터
- 각 타입마다 다른 포인터 변수를 선언하지 않고, 다른 포인터 타입으로 이용될 수 있는 하나의 포인터 변수를 선언할 수 있다.
- void* 은 비교 될 수 있다.
- 암시적으로 모든 포인터 타입은 void
*로 변경될 수 있다.
- 컴파일러가 해당 포인터가 어떤 오브젝트를 가리키는지 모르기 때문에 다른 연산은 안전하지 않다.
```
cout << (sizeof(void*) == sizeof(int*)); // true 출력

int array[] = {2,3,4};
void* ptr = array; // 암시적 변환
cout<< *array; // 2 출력
// *ptr; // 컴파일 에러!
// ptr + 2; // 컴파일 에러!
```