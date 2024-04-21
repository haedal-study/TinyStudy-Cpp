# Day-44

## Lambda Expressions
## Capture List

### Lambda Expression

- C++11 람다 표현식은 인라인 지역 범위 함수 객체임

```cpp
auto x = [capture clause](parameters){body}
```

- [capture clause]은 람다의 선언과 로컬 범위 인수가 캡처되는 방법(by-value, by-reference 등)을 표시
- 람다의 매개변수는 일반 함수 매개변수(선택 사항)
- 람다의 body는 일반 함수 body

```cpp
#include <iostream>
#include<algorithm>
using namespace std;

int main()
{
	int arr[] = {7, 2, 5, 1};
	//auto lambda = [](int a, int b) {return a > b; }; // named lambda

	//sort(arr, arr + 4, lambda);

	// unnamed lambda
	sort(arr, arr + 4, [](int a, int b) {return a > b; });

	for (auto i : arr) {
		cout << i << " ";
	}
	return 0;
}

```

### Capture List

- 람다 표현식은 두 가지 방식으로 람다 본문에 사용되는 외부 변수를 캡처
    - Capture by-value
    - Capture by-reference (외부 변수 값 수정 가능)
- 캡처 목록은 다음과 같이 전달
    - [] : no capture
    - [=] : 모든 변수를 by-value로 캡처
    - [&] : 모든 변수를 by-reference로 캡처
    - [var1] : var1만 by-value로 캡처
    - [&var2] : var2만 by-reference로 캡처
    - [var1, &var2] : var1은 by-value로, var2는 by-reference로 캡처
    
    ```cpp
    #include <iostream>
    #include<algorithm>
    using namespace std;
    
    int main()
    {
    	int limit = 4;
    	
    	auto lambda1 = [=](int value) {return value > limit; };       // by-value
    	auto lambda2 = [&](int value) {return value > limit; };       // by-reference
    	auto lambda3 = [limit](int value) {return value > limit; };   // "limit" by-value
    	auto lambda4 = [&limit](int value) {return value > limit; };  // "limit" by-reference
    	// auto lambda5 = [](int value) {return value > limit; };     // no capture
    	// 바깥쪽 함수의 지역 변수는 캡처 목록에 있지 않는 한 람다 본문에서 참조할수 없음
    	int arr[] = {7, 2, 5, 1};
    
    	cout << *find_if(arr, arr + 4, lambda1) << endl;
    	cout << *find_if(arr, arr + 4, lambda2) << endl;
    	cout << *find_if(arr, arr + 4, lambda3) << endl;
    	cout << *find_if(arr, arr + 4, lambda4) << endl;
    
    	return 0;
    }
    
    ```
    

### Capture List - Other Cases

- [=, &var1]은 참조로 캡처되는 var1을 제외하고 람다 바이값 본문에 사용된 모든 변수를 캡처
- [&, var1]은 값으로 캡처되는 var1을 제외하고 람다 바이 레퍼런스 본문에서 사용된 모든 변수를 캡처
- 람다 표현식은 변수가 constexpr인 경우 변수를 캡처하지 않고 읽을 수 있음
    
    ```cpp
    #include <iostream>
    #include<algorithm>
    using namespace std;
    
    int main()
    {
    	constexpr int limit = 4;
    	
    	auto lambda = [](int value) {return value > limit; };     // no capture
    	// constexpr 변수는 no-capture 람다 함수여도 읽을 수 있음
    	int arr[] = {7, 2, 5, 1};
    
    	cout << *find_if(arr, arr + 4, lambda) << endl;
    
    	return 0;
    }
    ```
