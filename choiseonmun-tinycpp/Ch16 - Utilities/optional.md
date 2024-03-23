- C++17
- 값의 부재를 나타냄

```cpp
#include <optional>

using namespace std;

// 1. 생성
optional<string> optionalValue = "abcd";
optioanl<string> optioanlValue = {}; // std::nullopt

// 2. 값 유무 확인
if (optionalValue) cout << "값이 있다.";
if (optionalValue.has_value()) cout << "값이 있다.";

// 3. 값 확인
// value()의 경우 값이 없으면 bad_optional_access 예외 발생
optionalValue.value();
optioanlValue.value_or("asada");
*optionalvalue;
```


# 참고자료
- https://en.cppreference.com/w/cpp/utility/optional