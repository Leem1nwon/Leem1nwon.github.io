---
layout: single
title: "Compile, Linking, Build Process, Errors"
categories: Basic_C++
---

# C++ 프로그램의 구조

이 문서는 C++ 프로그램의 빌드 프로세스와 다양한 오류(컴파일러 에러, 경고, 링커 에러, 런타임 에러, 논리적 오류)에 대해 설명한다.

--------------------------------------------------

## 1. 빌드 프로세스

### 1.1 프로그래밍 언어
- 고수준의 소스코드를 작성하는 데 사용
- **Human-readable**

### 1.2 오브젝트 코드
- **Machine-readable**: 컴퓨터가 실행할 수 있는 코드

### 1.3 컴파일러
- 소스코드를 오브젝트 코드로 변환하는 도구

### 1.4 링커
- 오브젝트 코드를 실행 파일(exe)로 연결하는 도구

### 1.5 테스트 & 디버깅
- 프로그램의 오류를 찾고 수정하는 과정

### 1.6 IDE (Integrated Development Environment)
- 텍스트 에디터, 컴파일러, 링커, (디버거)를 통합한 개발 환경

![C++ Build Process](../images/2025-01-26-Introduction%20and%20Variable/cpp_build_process.png){width=500px}

--------------------------------------------------

## 2. Compiler Errors

### 2.1 프로그래밍 언어 규칙 위반

#### 2.1.1 문법적 오류 (Syntax Errors)
```cpp
std:cout << "Errors << std::endl;
return 0;
```

#### 2.1.2 의미 오류 (Semantic Errors)
```cpp
int a = 5;
string b = "Hello World";

a + b;
```

### 2.2 Compiler Warnings
- 코드에 잠재적인 문제가 있을 때 발생 (빌드는 가능하지만 무시하면 안됨)
```cpp
int distanceDriven;
std::cout << distanceDriven;
```

### 2.3 Linker Errors / Runtime Errors

#### 2.3.1 링크 에러 (Linker Errors)
- obj 파일의 연결 과정에서, 필요한 라이브러리나 obj 파일을 찾을 수 없을 때 발생

#### 2.3.2 런타임 에러 (Runtime Errors)
- 프로그램 실행 도중 발생하는 오류 (예: 0으로 나누기, 파일 없음, 메모리 부족)
- 프로그램이 crash 될 수 있으며, 예외 처리를 통해 해결 가능

## 3. Logical Errors
- 프로그램의 동작에 관한 논리적 오류로, 작성자의 실수가 원인
```cpp
if (age >= 19) {  // 기준 연령이 다를 수 있음
    std::cout << "You can drink!";
}
```

--------------------------------------------------

## 참고 자료
이 문서 작성에는 [YouTube Playlist: C++ Programming][playlist]를 참고했습니다.

[playlist]: https://www.youtube.com/playlist?list=PLMcUoebWMS1nzhlx-NbD4KBGEP1UCUDF_