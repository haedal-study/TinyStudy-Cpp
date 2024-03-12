# Hello World
```
int main() {
    //C Code
    printf("Hello World\n");

    //C++ Code
    std::cout << "Hello World\n";
}
```
- printf
    - 표준 출력물 프린트
- cout
    - 표준 출력 스트림을 나타냄
<br></br>
# I/O Stream (왜 I/O Stream을 선호하는가)
- 타입 안정성
    - I/O Stream은 실제 전달된 개체와 일치해야 하는 % 토큰이 없음
- 확장성
    - 새로운 사용자 정의 유형을 I/O Stream에 전달 가능
- 비슷한 성능
<br></br>
# 일반적인 C 에러
```
//파라미터 불일치
printf("long phrase %d long phrase %d, 3);

//잘못된 형식 지정자 사용
int a = 3;
printf("%f", a);

//선두 공백도 문자로 취급
scanf(" %c", &var);
```
<br></br>
# std::print
- C++23에 도입
- 향상된 버전의 printf의 기능
<br></br>
## 자료
- [Modern C++ Programming - Basic Concepts I](https://github.com/federico-busato/Modern-CPP-Programming/blob/master/02.Basic_Concepts_I.pdf)
- [printf/scanf와 cout/cin의 차이](https://codedamn.com/news/c/difference-between-printf-scanf-and-cout-cin-in-c)
- [printf와 cout의 차이](https://stackoverflow.com/questions/2872543/printf-vs-cout-in-c)
- [printf와 cout의 차이 2](https://qna.programmers.co.kr/questions/1239/c%EC%97%90%EC%84%9C-printf%EB%9E%91-cout%EC%9D%80-%EB%AC%B4%EC%8A%A8-%EC%B0%A8%EC%9D%B4%EA%B0%80-%EC%9E%88%EB%82%98%EC%9A%94)
- [형식 사양 구문](https://learn.microsoft.com/ko-kr/cpp/c-runtime-library/format-specification-syntax-printf-and-wprintf-functions?view=msvc-170)
- [형식 필드 문자](https://learn.microsoft.com/ko-kr/cpp/c-runtime-library/format-specification-syntax-printf-and-wprintf-functions?view=msvc-170#type)