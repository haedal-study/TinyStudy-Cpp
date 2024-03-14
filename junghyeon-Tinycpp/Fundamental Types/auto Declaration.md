# auto Keyword
- C++11부터 사용
- 컴파일러에 의해 자동으로 추론
- 유지보수성과 복잡한 유형 정의를 숨기는데 매우 유용
- 가독성이 떨어질 수 있음
- C++11/C++14에서 함수의 반환형으로 사용 가능
    ```
    auto g(int x) -> int { return x * 2; }                  //C++11
    auto g2(int x) -> decltype(x * 2) { return x * 2; }     //C++11
    
    auto h(int x) { return x * 2; }                         //C++14
    ```
- C++20에서 함수의 매개변수에도 사용 가는
    ```
    void f(auto x) {}
    ```
## 자료
- https://github.com/federico-busato/Modern-CPP-Programming/blob/master/02.Basic_Concepts_I.pdf
