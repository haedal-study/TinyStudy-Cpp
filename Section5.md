#5 Basic Concepts IV - Memory Management

-technical term(?)
    - {}; brace ->      curly brackets,
    - [];               square brackets,
    - () parentheses -> round brackets,
    - <>                angle brackets,

**Heap and Stack**

   <img src="/images/MemoryStructure.png" width="320" height="180">

   - stack과 heap의 차이를 요약하면 다음과같습니다
    - 정렬방식
         -둘다 인접(contigous)메모리 방식입니다.(메모리 공간을 순서대로 정렬하는 방식을 말합니다.)
    - 최대 사이즈  
         -스택은 상대적으로 적은 메모리를 사용합니다. 리눅스의 경우 8MB, 윈도우의 경우 1MB정도를 할당받습니다. 지역(임시)변수만을 할당받기 떄문입니다.
         - 시스템 메모리 만큼 할당가능하며, 크고 지속되는 정보를 할당하기 적합합니다.
    - 메모리 초과의 경우
        -  스택의 경우 시스템 크래쉬(스택 오버플로우)가 발생합니다. 기본적으로 초과하는 경우 지역변수할당과 리턴주소를 메모리 용량을 더 확보하지 않기 때문입니다.
        - 힙 메모리의 경우, 즉각적인 프로그램 크래쉬가 발생하지는 않지만, 메모리 할당은 실패 할 수 있습니다. 이런 경우  예외처리 오류나 널포인터를 반환합니다.
    - 메모리 할당
        - 스택의 경우 **컴파일 타임** 에 메모리 할당이 일어납니다. 즉 사이즈와 생명주기는 컴파일타임에 결정됩니다. 다만 런타임에 크기가 바뀜으로 힙과함께 동적할당영역이라고 부릅니다. 
        - 반면힙의 경우는 런타임에 할당됩니다.
    - Thread view
        - 스레드 관점에서, 모든 스레드는 각각 자신의 스택을 가집니다. 즉 각 스레드는 각각의 지역 변수들을 가집니다
        - 힙은 모든 스레드가 공유하는 영역입니다. 여러개의 스레드가 접근하는 경우, 적절한 동기화로 스레드끼리의 충돌을 피해야 합니다. 
        
    -멸망예시
        -아래 코드는 출력이 되긴합니다(컴파일 에러 안남) 하지만 Illegal memory access입니다!
        ```
        int* f() {
        int array[3] = {1, 2, 3};
        return array;
        }
        int* ptr = f();
        cout << ptr[0]; // Illegal memory access!! A
        ``` 

        ```
        //- 1. xyz가 범위를 벗어나면 메모리 해제가 발생합니다. 근데 이 함수에서 str은 xyz 배열을 가리키도록 합니다
        //- 2. str은 해제가 된 메모리를 가리키고 있습니다. 즉 Dangling Pointer상태입니다.
        //- 3 따라서 str은 Dangling Pointer이므로, 언제든지 프로그램의 다른 부분에 의해 덮어씌워질 수 있습니다. 

        void g(bool x) {
        const char* str = "abc";
        if (x) {
            char xyz[] = "xyz";
            str = xyz;
        }
        // If "x" is true, 'str' points to 'xyz' which goes out of scope here
        std::cout << str;
        }
        ```

- heap의 메모리할당 키워드
    - new/new[] , delete/delete[]는 동적 메모리 할당/해제를 나타내는 키워드입니다 