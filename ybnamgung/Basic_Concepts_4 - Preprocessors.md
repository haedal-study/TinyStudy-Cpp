# Basic_Concepts_4 - Preprocessors

## Preprocessors

모든 상태는 ‘#’로 시작한다.

- #include "my file.h": 현재 파일에 코드를 삽입한다.
- • #define MACRO <expression>: 새로운 매크로를 정의한다.
- #undef MACRO: 매크로를 정의 해제한다. (매크로는 안전성을 위해 가능한 빨리 해제되어야 한다.)
- 여러 줄의 전처리: 한 줄의 끝에 \를 사용한다.
- 들여쓰기: # define