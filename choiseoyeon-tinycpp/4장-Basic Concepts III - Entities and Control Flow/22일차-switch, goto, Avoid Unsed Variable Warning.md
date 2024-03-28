# switch
- `switch`문은 int, char, enum class, enum 문을 판별하여 해당하는 케이스 값과 관련된 코드를 실행시킨다.
- 스위치 스코프
	- 각 케이스마다 스코프가 다르기 때문에, 위 스코프에서 정의 했다고, 가져다가 사용할 수 없다.
- fall-through
	- return 문, 혹은 break 문으로 스코프를 나올 수 있다.
	- 나오지 않는다면 다음 케이스문도 실행하게 됨
	- 이것 역시 워닝이 뜰 수 있어서 `[[fallthrough]]`어트리뷰트를 통해 방지 할 수 있다.
- 초기화문을 통한 흐름 제어
	- 흐름 제어 본문에서만 확징 할 수 있는 변수들의 스코프를 평가하고 제한하기 전에 복잡한 행동들을 단순화 하기 위해 사용된다.
```
//C++17 if문
if(int ret = x + y; ret < 10)
	cout << ret;

// C++17 switch 문
swtich(auto i = f(); x)
{
	case 1: return i + x;
}

// C++20 range-for loop 문
for(int i = 0; auto x; {'A', 'B', 'C'})
	cout<< ++i << ":" << x << " ";
	// 1:A 2:B 3:C
```

# goto
- 다음과 같은 상황에서 유용할 수 있다.
```
bool flag = true;
for(int i = 0; i < N && flag; i++)
{
	for(int j = 0; j < M && flag; j++)
	{
		if(조건)
			flag = false;
	}
}

// goto 사용
for(int i = 0; i < N; i++)
{
	for(int j = 0; j < M; j++)
	{
		if(조건)
			goto LABEL;
	}
}
LABEL : ;

// 사실 가장 좋은 방법은...
bool func(int N, int M)
{
	for(int i = 0; i < N && flag; i++)
	{
		for(int j = 0; j < M && flag; j++)
		{
			if(조건)
				return false;
		}
	}
	return true;
}
```
- 안쓰는 게 좋음... 스파게티 코드...
# Avoid Unused Variable Warning
- 대부분의 컴파일러 이슈는 어떤 변수가 사용되지 않았을 때 이다.
- C++17에서는 `[[maybe_unused]]` 키워드를 통해 이런 워닝을 방지 할 수 있다.
```
int f(int value)
{
	[[maybe_unused]] int x = value;
#if defined(ENABLE_SQUARE_PATH)
	return x * x;
#else
	return 0;
#endif
}
```
