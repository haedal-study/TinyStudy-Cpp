## 2.1 Hello World
- C 언어에서의 printf
```
int main() {
printf("Hello World!\n");
}
```

- cout 은 C++의 표준 출력 스트림입니다.
  - cout은 iostream의 일부이며 , Character output의 약자입니다. 
  - 스트림이란 프로그램에서 출력자치로 데이터가 '흐르는' 방식을 일컫습니다. 즉 데이터가 전송되거나 수신되는 방법을 추상화 한 것 입니다.

```
#include <iostream>
int main() { 
        std:cout << "Hello World!\n";
    }
```

- global std를 이용해 cout을 사용한 경우

``` 
#include <iostream>
using namespace std; 
int main() {
    cout << "Hello World!\n";
}

- 아래는 입출력 스트림에 대한 예제 코드 입니다

```
#include <iostream>
int main() {
    int a = 4;
    double b = 3.0;
    char c[] = "hello";
    std::cout << a << "" << b << "" << C << "\n" 
} 
```