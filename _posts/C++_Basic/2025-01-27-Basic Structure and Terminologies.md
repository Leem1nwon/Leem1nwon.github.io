---
layout: single
title: "Basic Structure, Terminology, Memory and Variable"
categories: Basic_C++
---

## 1. C++ keyword, Identifier, Operator

### 1.1 키워드
- 약 90개의 키워드가 존재 (예: 변수 타입, if, for 등)
- 언어 자체에서 예약된 단어들로, 특별한 의미를 갖는다.

### 1.2 식별자
- 변수, 함수, 타입 등 개발자가 지정하는 이름
- 대소문자를 구분함
- 가독성을 위해 의미 있는 이름을 사용해야 함

### 1.3 연산자
- 예: +, -, *, /, >>, <<, ::, ...

---

## 2. C++ Preprocessor

### 2.1 전처리기 (Preprocessor)
- 컴파일 이전에 처리됨 (즉, 소스코드의 텍스트 치환)
- "#" 으로 시작하며, 대표적으로 다음과 같은 지시문이 있다.
  - `#include`
  - `#ifdef`, `#ifndef`, `#define`, `#undef`
  - `#pragma`

### 2.2 #include
- 다른 파일의 내용을 포함시키며, 단순 복사 붙여넣기와 같은 역할을 한다.
  - 예: `#include <iostream>`은 iostream의 코드 전체를 포함하는 것과 동일

### 2.3 #define
- Platform independence, 코드 단축, 디버그 등 다양한 목적으로 사용됨

---

## 3. main() Function

### 3.1 메인 함수의 역할
- 모든 C++ 프로그램은 하나의 `main()` 함수를 가져야 함
- 프로그램의 진입점 (프로그램이 실행되면 가장 먼저 실행되는 함수)
- 리턴값 0은 올바른 실행을 의미함

```cpp
int main() {
    // code
}
```

---

## 4. C++ Namespace

### 4.1 개요
- 네임스페이스는 이름 충돌을 방지하기 위해 사용된다.
- 외부 라이브러리나, 프로젝트 내 다른 함수/변수들과의 이름 충돌을 피하기 위함.

### 4.2 예제
```cpp
#include <iostream>

int main() {
    int favoriteNumber;
    std::cout << "Enter ...";
    std::cin >> favoriteNumber;
    std::cout << "Amazing!! ..." << std::endl;
}
```
- `std::cout`에서 **std**는 네임스페이스이며, `::`는 scope resolution operator.

### 4.3 using namespace
- 특정 네임스페이스 내의 이름들을 사용할 때 `using namespace`를 선언할 수 있으나, 남용하면 오히려 충돌 위험이 커진다.

![namespace](../images/2025-01-27-Basic%20Structure%20and%20Terminologies/namespace.png)

---

## 5. Basic I/O

### 5.1 표준 입출력 스트림
- **cout**과 **<<**:  
  C++의 표준 출력 스트림, 삽입 연산자를 사용해 순차적인 출력을 수행한다.
  ```cpp
  int age = 20;
  std::cout << "My age is " << age;
  ```
  - 줄바꿈을 원할 경우 `endl` 또는 `"\n"`을 사용한다.

- **cin**과 **>>**:  
  C++의 표준 입력 스트림, 추출 연산자를 사용해 순차적인 입력이 가능하다.
  ```cpp
  std::cin >> myAge >> myHeight;
  ```

![stream](../images/2025-01-27-Basic%20Structure%20and%20Terminologies/stream.png){width=500px}

---

## 6. 변수와 메모리

### 6.1 Bit, Byte, Hex
- **Bit**: 0 또는 1의 값을 가짐.
- **Byte**: 1 byte = 8 bit  
  - 예: 1 byte로 0~255의 양의 정수를 표현 가능.
- **Hexadecimal (Hex, 16진수)**:  
  16진수 2자리로 1 byte를 보기 쉽게 표현하며, 앞에 `0x`를 붙여 사용한다.
  
![hex](../images/2025-01-27-Basic%20Structure%20and%20Terminologies/hex.png){width=500px}

### 6.2 Memory
- **메모리**: 읽고 쓸 수 있는 바이트 단위 공간의 집합 (주로 RAM을 의미)
- 비유:  
  - 메모리를 호텔에 비유하면, 한 층(byte)마다 8개의 방(bit)이 있으며, 각 방은 0 또는 1의 상태를 가진다.
  - 이진 데이터를 보기 쉽게 표현하기 위해 Hex를 사용한다.
- 메모리의 각 바이트는 고유한 **주소**를 가지며, 주소도 Hex로 표현된다.

| 타입   | char   | short  | int         |
|:------:|:------:|:------:|:-----------:|
| 용량   | 1 byte | 2 byte | 4 byte      |
| 예: 10 저장 시 | 0x0A  | 0x000A | 0x0000000A |

### 6.3 Variable

#### 6.3.1 변수란?
- **정의:**  
  메모리의 주소에 붙이는 이름.
- 변수를 정의하면 해당 메모리 공간이 확보되고, 그 공간에 값을 대입하여 사용할 수 있다.
- **예시:**  
  ```cpp
  int age = 21;
  ```
- 만약 변수가 없다면, 직접 메모리 주소를 사용해야 하므로 코드 작성이 매우 불편해진다.
  ```cpp
  int 0xFA84 = 10;
  0xFA84 = 0xFA90 + 6;
  ```
  
![variable and memory](../images/2025-01-27-Basic%20Structure%20and%20Terminologies/variable%20and%20memory.png){width=400px}

#### 6.3.2 변수의 정의
- C++에서는 변수를 정의할 때 반드시 타입을 명시해야 한다.
  ```cpp
  char a;
  int age;
  double rate;
  std::string name;
  ```

#### 6.3.3 변수의 초기화
- 변수를 정의하면서 초기값을 설정하는 것을 초기화라 한다.
  ```cpp
  char a = 10;
  int age = 21;
  double rate = 0.85;
  std::string name = "Hyungki Kim";
  ```

#### 6.3.4 변수의 사용
- 변수 이름을 사용하여 메모리의 값을 읽거나 쓴다.
  ```cpp
  a = 20;          // a 변수에 20을 저장
  printf("%d", a); // a 변수의 값을 출력
  ```
- 간단한 규칙:
  - 이름 앞에 타입이 있으면 → 변수 정의(초기화)
  - 이름만 있으면 → 변수 사용

---

## 7. 변수의 타입

### 7.1 Integer
- 정수를 표현  
![integer](../images/2025-01-27-Basic%20Structure%20and%20Terminologies/integer.png){width=600px}

### 7.2 Floating Point
- 실수를 표현  
![floating point](../images/2025-01-27-Basic%20Structure%20and%20Terminologies/floating%20poin.png){width=600px}

### 7.3 Boolean
- true/false를 표현  
![boolean](../images/2025-01-27-Basic%20Structure%20and%20Terminologies/boolean.png){width=600px}

### 7.4 sizeof 연산자
- 타입이나 변수의 바이트 단위 크기를 반환한다.
  ```cpp
  sizeof(int);          // int의 크기
  sizeof(double);       // double의 크기
  sizeof(favoriteNumber); // 변수의 크기
  ```

---

## 8. 상수

### 8.1 상수란?
- 변수와 유사하지만, 초기화 이후 변경할 수 없는 값.
- 사용 목적: **실수 방지 및 프로그램의 견고함**
- 협업 시 중요한 역할을 함.

### 8.2 상수의 종류
- 리터럴 상수: 12, 3.14, "Minwon"
- 선언 상수: `const int favoriteNumber = 18;`
- 상수 표현: `constexpr int age = 20;`
- 열거형 (enum)
- 정의 상수: `#define pi 3.1415926`

---

## 참고 자료
이 문서 작성에는 [YouTube Playlist: C++ Programming][playlist]를 참고했습니다.

[playlist]: https://www.youtube.com/playlist?list=PLMcUoebWMS1nzhlx-NbD4KBGEP1UCUDF_
-->
