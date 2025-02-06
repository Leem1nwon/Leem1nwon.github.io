---
layout: single
title: "Compile, Linking, Build Process, Errors"
---

# C++ 프로그램의 구조

## 빌드 프로세스

#### 1. 프로그래밍 언어
- 고수준의 소스코드 작성에 사용
- **Human-readable**

#### 2. 오브젝트 코드
- **Machine-readable**: 컴퓨터가 실행할 수 있는 코드

#### 3. 컴파일러
- 소스코드를 오브젝트 코드로 변환하는 도구

#### 4. 링커
- 오브젝트 코드를 실행 파일(exe)로 변환하는 도구

#### 5. 테스트 & 디버깅
- 프로그램에 존재하는 오류를 찾고, 수정하는 과정

#### 6. IDE (Integrated Development Environment)
- 텍스트 에디터 + 컴파일러 + 링커 + (디버거)

![C++ Build Process](../images/2025-01-26-Introduction%20and%20Variable/cpp_build_process.png){width=500px}

---

## Compiler Errors

#### 1. 프로그래밍 언어의 규칙을 위반하는 경우
##### 문법적 오류: 코드 자체의 오류
```cpp
std:cout << "Errors << std::endl;
return 0;
```
##### 의미 오류
```cpp
int a = 5;
string b = "Hello World";

a + b;
```
## Compiler Warnings
코드에 잠재적인 문제가 있을 것으로 예상될 때
빌드는 가능하지만, 무시하면 안됨!
```cpp
int distanceDriven;
std::cout << distanceDriven;
```
### Linker Errors / Runtime Errors

#### 링크 에러
- `obj` 파일의 링크 과정에서 오류가 있을 경우
- 주로 라이브러리 또는 `obj` 파일을 찾을 수 없는 경우

#### 런타임 에러
- 프로그램이 실행 도중 발생하는 오류
- 프로그램이 "뻗어버린다(?)"라고 표현하기도 함
  - Divided by zero, file not found, out of memory, etc...
  - 프로그램의 crash
- 예외 처리를 통해 문제 발생에 따른 처리를 할 수 있음!

## Logical Errors
- 프로그램의 동작에 관한 논리적 오류
- 프로그램 작성자의 실수가 원인 
- 테스트 과정을 통해 찾아내고, 수정해야 함!
```cpp
if (age >= 19) {                    //19세가 아닌, 다른 나이가 기준일 수도 있다.
    std::cout << "You can drink!"; 
}
```

---
### 참고 자료
이 문서 작성에는 [YouTube Playlist: C++ Programming][playlist]를 참고했습니다.

[playlist]: https://www.youtube.com/playlist?list=PLMcUoebWMS1nzhlx-NbD4KBGEP1UCUDF_