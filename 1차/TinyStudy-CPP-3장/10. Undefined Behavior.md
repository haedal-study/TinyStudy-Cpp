### Undefined Behavior가 발생하는 경우
---

* signed integer에서 양수보다 음수가 더 큰 걸 간과했을 때
```cpp
int y = std::numeric_limits<int>::min() * -1; // -2ˆ31 * -1 
cout << y; // 음수의 최대값보다 양수의 최대값이 적으므로 오버플로우가 발생함.
```

* 타입의 범위보다 큰 값을 할당할 때

* signed integer에 bit-wise연산을 수행할 때
```cpp
int y = 1 << 12;
```

* unsigned integer의 비트 수보다 큰 값으로 bit-wise 연산을 수행할 때


*Overflow, Wraparound를 막기 위해 계산이 수행되기 이전에 Undefined Behavior인지 확인하는 것이 좋다.*

