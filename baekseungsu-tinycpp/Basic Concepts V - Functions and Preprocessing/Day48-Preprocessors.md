# Day-48

## Preprocessing
## Preprocessors

### Preprocessing and Macro

- 전처리기 지시어는 컴파일하기 전에 컴파일러에게 소스 코드를 해석하는 방법을 알려주는 해시기호(#)가 앞에 오는 라인임
- 매크로는 전처리기 지시어로, 코드의 나머지 부분에서 식별자가 있는 경우 “replacement”로 대체함
- 매크로 확장을 사용하지 않거나 가능한 적게 사용해야함
    - 매크로는 직접 디버깅할 수 없음
    - 매크로 확장은 예기치 않은 부작용을 발생시킬 수 있음
    - 매크로에는 네임스페이스나 범위가 없음

### Preprocessors

- ‘#’로 시작하는 모든 구문
    - #include “my_file.h”
        - 현재 파일에 코드를 삽입
            
            ```cpp
            #include <filename>
            // <>는 컴파일러와 함께 제공되는 헤더 파일을 include할 때 사용
            
            #include "filename"
            // ""는 소스 파일이 있는 디렉터리에서 헤더 파일을
            // include 하도록 전처리기에게 지시
            // 일반적으로 자신이 작성한 헤더파일을 include할 때 사용
            ```
            
    - #define MACRO <expression>
        - 새 매크로 정의
            
            ```cpp
            #define MY_NUM 9
            
            std::cout << "My Num : " << MY_NUM << std::endl;
            // 출력 9
            ```
            
    - #undef MACRO
        - 매크로 정의 해제
        - 매크로는 안전상의 이유로 가능한 한 빨리 정의 해제 해야함

### Conditional Compiling

- 조건부 컴파일 전처리 지시자를 사용하면 컴파일할 조건이나 컴파일 하지 않을 조건을 지정할 수 있음
    
    ```cpp
    #define PRINT_FIRST_NAME
    
    #ifdef PRINT_FIRST_NAME
    std::cout << "SeungSu" << std::endl;
    #endif
    
    // 정의되어있지 않기 때문에 코드는 무시됨
    #ifdef PRINT_LAST_NAME
    std::cout << "Baek" << std::endl;
    #endif
    ```
    
    ```cpp
    #ifndef PRINT_NAME
    std::cout << "name" << std::endl;
    #endif
    
    // ifndef는 ifdef의 반대, 식별자가 아직 정의되지 않았는지 확인
    // PRINT_NAME은 정의되지 않았기 때문에 "name" 출력
    ```
