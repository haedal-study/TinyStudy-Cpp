# C++ Philosophy
- 성능
    - 성능을 희생하지 말 것
    - **Zero Overhead** 원칙
        - 사용하지 않는 것에 대해 비용을 지불하지 않음
- 타입 안정성
    - 가능한 컴파일 타임에 안정하게 실행
    - 정적으로 타입 지정된 언어
        - 주석 사용
        - 컴파일러 최적화 및 런타임 효율성 촉진
        - 사용자가 자체 타입 시스템을 정의할 수 있음
- 프로그래밍 모델
- 예측 가능한 런타임
- 낮은 리소스
- 정적 분석에 적합함
- 휴대성이 뛰어남
<br></br>
## 타입 안정성 이슈 (C++, C#)
- C++
    - new 연산자는 유형의 포인터를 반환
        - malloc은 공백 포인터를 반환
    - 가상 함수 및 템플릿을 사용하여 void 포인터 없이 다형성을 달성할 수 있음
    - 동적 캐스팅과 같은 Safer casting operator
- C#
    - 타입이 지정되지 않은 포인터를 사용하지만 "unsafe" 키워드를 사용하여 컴파일 단계에서 막음
    - 런타임 캐스트 검증을 고유하게 지원
    - 검증 방법
        - "as" 키워드 사용
        - 예외를 던지는 C 스타일 캐스트를 사용
<br></br>
## 자료
- https://github.com/federico-busato/Modern-CPP-Programming/blob/master/01.Introduction.pdf
- https://en.cppreference.com/w/cpp/language/Zero-overhead_principle
- https://tango1202.github.io/principle/principle-zero-overhead/
- https://en.wikipedia.org/wiki/Type_safety
