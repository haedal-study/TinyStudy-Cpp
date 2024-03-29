# If 구문
- 지정된 조건이 true면 첫 번째 분기를 실행하고 그렇지 않으면 두 번째 분기를 실행
- 삼항 연산자
    ```
    <condition> ? <expression1> : <expression2>;
    ```
    - `<expression1>`과 `<expression2>`는 같은 값을 반환하거나 변환가능한 타입
<br></br>
# for, while 구문
- for 구문
    - 반복 횟수를 알 경우 사용
    ```
    for ([init]; [cond]; [increment]) {
        ...
    }
    ```
- while 구문
    - 반복 횟수를 모를 경우 사용
    ```
    while (cond) {
        ...
    }
    ```
- do while 구문
    - 반복 횟수는 모르지만 반드시 한 번은 실행해야 할 경우 사용
    ```
    do {
        ...
    } while (cond);
    ```
# for 구문의 특징
- C++은 루프 안에 정의를 허용
    ```
    for (int i = 0, k = 0; i < 10; i++, k += 2)
        ...
    ```
- 무한 루프
    ```
    for (;;)            // also while(true);
        ...
    ```
- Jump Statement
    ```
    for (int i = 0; i < 10; i++) {
        if (<condition>)
            break;      // exit from the loop
        if (<condition>)
            continue;   // continue with a new iteration and exec. i++
        return;         // exit from the function
    } 
    ```
# 범위 기반 for 루프
- C++11에 도입
- 더 안전함
- 루프의 시작, 끝 및 증분을 지정하지 않음
```
for (int v : { 3, 2, 1 })   // INITIALIZER LIST
    cout << v << " ";       // print: 3 2 1

int values[] = { 3, 2, 1 };
for (int v : values)        // ARRAY OF VALUES
    cout << v << " ";       // print: 3 2 1

for (auto c : "abcd")       // RAW STRING
    cout << c << " ";       // print: a b c d
```
- C++17부터 구조체 결합을 위한 개념 확장
```
struct A {
int x;
int y;
};

A array[] = { {1,2}, {5,6}, {7,1} };
for (auto [x1, y1] : array)
    cout << x1 << "," << y1 << " ";     // print: 1,2 5,6 7,1
```
# 자료
- https://github.com/federico-busato/Modern-CPP-Programming/blob/master/04.Basic_Concepts_III.pdf