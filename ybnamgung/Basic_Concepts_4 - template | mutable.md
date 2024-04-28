# Basic_Concepts_4 - template | mutable

## template

C++20에서는 람다 표현식이 template과 requires 절을 지원한다.

```cpp
auto lambda = []<typename T>(T value)
				requires std::is_arithmetic_v<T> {
		return value * 2;
};
auto v = lambda(3.4); // v: 6.8 (double)

struct A{} a;
// auto v = lambda(a); // 컴파일 에러
```

## mutable

람다 캡처는 상수 값으로 이루어진다.
mutable 한정자는 람다가 값에 의해 캡처된 매개변수를 수정할 수 있게 한다.

```cpp
int var=1;
auto lambda1 = [&](){ var = 4; };
lambda1();
cout << var << endl; // print '4'
    
// auto lambda2 = [=](){ var = 3; };
// lambda operator() is const ok
    
auto lambda3 = [=]() mutable { var = 3; }; // ok
lambda3();
cout << var << endl; // print '4', lambda3 captures by-value
```