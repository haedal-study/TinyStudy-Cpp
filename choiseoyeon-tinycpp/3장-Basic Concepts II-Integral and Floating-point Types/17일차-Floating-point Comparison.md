문제점:
```
cout << (0.11f + 0.11f < 0.22f); // true 출력
cout << (0.1f + 0.1f > 0.2f); // true 출력
```
> 왜 두 부동 소수점을 합치면 값이 정확하게 나오지 않을까?

"절대 오차 한계를 사용하지 말 것!"
- 고정된 입실론은 작아보이지만, 아주 작은 차이가 나는 숫자를 비교할 때는 엄청나게 클 수 있다.
- 비교되는 수가 엄청나게 크다면, 입실론은 너무 작아져서, 가장 작은 반올림 오류에 비해도 너무 클 수 있다. 그래서 항상 false가 나올 수 있다.
	- 입실론보다 오류가 크기 때문에, 단순히 오류인지 확인하기 어려움
```
bool areFloatNearlyEqual(float a, float b)
{
	// 사용자에 의해 입실론이 고정됨
	if(std::abs(a-b) < epsilon)
		return true;
	return false;
}
```

해결책: 상대 오차 사용하기(abs(a-b)/b < epsilon)
```
bool areFloatNearlyEqual(float a, float b)
{
	// 입실론이 고정됨
	if(std::abs(a-b)/b < epsilon)
		return true;
	return false;
}
```
위의 수식을 사용했을 때 문제들:
- a=0, b=0일 때, 항상 나누기는 0.0/0.0일 것이고, 따라서 무조건 false 반환
- b=0 일 때, 항상 나누기는 abs(a)/0.0일 것이고, 따라서 무조건 false 반환
- a 와 b가 아주 작을 때,  결과는 true 이어야하지만, b에 의해 나눠져서 이상한 값이 나올 수 있음
- 항상 b로만 나눠야 함

해결책: 다음과 같음
```
bool areFloatNearlyEqual(float a, float b)
{
	constexpr float normal_min = std::numeric_limits<float>::min();
	constexpr float relative_error = <user_defined>

	if(!std::isfinite(a) || !isfinite(b))
		return false;
	float diff = std::abs(a-b);

	if(diff <= normal_min)
		return true;

	float abs_a = std::abs(a);
	float abs_b = std::abs(b);
	return (diff/std::max(abs_a, abs_b)) <= relative_error;
}
```