# Day-39

## sizeof Operator

### sizeof

- sizeof는 변수 또는 데이터 타입의 크기를 바이트 단위로 결정하는 컴파일 타임 연산자
- sizeof는 value의 크기를 반환
- sizeof는 절대 0을 반환하지 않음
- sizeof(char)는 항상 1을 반환
- 구조체에 적용하면 내부 패딩도 고려
- 참조에 적용하면 결과는 참조된 형의 크기
- sizeof(불완전한 타입) → 컴파일 에러
- sizeof(비트필드멤버) → 컴파일 에러

### sizeof - Pointer

```cpp
sizeof(int); // 4 bytes
sizeof(int*) // 8 bytes, 64-bit 운영체제에서
sizeof(void*) // 8 bytes, 64-bit 운영체제에서
sizeof(size_t) // 8 bytes, 64-bit 운영체제에서)
```

### sizeof - struct

- 구조체의 크기는 구조체를 구성하는 요소들에 의해 정해짐
- 구조체는 구조체가 포함하고 있는 요소들중 가장 큰 값을 기준으로 그 값의 배수만큼 크기를 가짐
    
    ```cpp
    struct s1{
    	char flags; // 1 byte
    	short count; // 2 bytes
    	int a; // 4 bytes
    }; // 1 + 2 + 4 = 7이니까 7bytes?
    
    sizeof(s1) // 8 bytes
    ```
    
- 자료의 배치에 따라서 구조체의 크기가 달라질 수 있음
    
    ```cpp
    struct s1{
    	char flags; // 1 bytes
    	int a; // 4 bytes
    	short count; // 2 bytes
    }; // 1 + 2 + 4 = 7이니까 7bytes?
    
    sizeof(s1) // 12 bytes
    ```
    
    - CPU가 자료형을 저장할때 효율을 위해 자료형 크기의 해당하는 주소에 위치시키기 때문

### sizeof - Reference and Array

```cpp
char a;
char& b = a;
sizeof(&a); // 8 bytes (포인터 크기)
sizeof(b); // 1 byte, sizeof(char)과 동일
           // 참조는 포인터가 아님에 주의할것

struct A{};
sizeof(A); // 1 반환, sizeof는 절대 0을 return 하지 않음

A arr1[10];
sizeof(arr1); // 1 반환, 빈 구조 배열

int arr2[0];  // only gcc
sizeof(arr2); // 0 : 특별한 경우
```
